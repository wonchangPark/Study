light weight kubernetes
=======================

## 라즈베리 파이에 k3s를 올려서 컨테이너들을 관리
### 라즈베리 파이 설치
Raspberry Pi 4 Model B 를 사용.  
raspberrypi.org에서 raspberry pi os를 맥에 깔고 sd 카드를 통해 write 해준 다음에 라즈베리 파이에 꽂았다.  
그냥 순전히 서버로 사용하기 때문에 마우스와 키보드는 사용하지 않으므로 접근 방법을 ssh(secure shell)을 이용했다.   
따라서 raspberry pi os 가 깔린 sd 카드 안에 설정을 한다.   
mac os의 경우는 /Volumes/boot에 sd카드가 연결된다. 여기에 ssh라는 빈 파일과 supplicant.conf라는 파일이 필요하다.   
```
touch ssh
vi supplicant.conf
// supplicant.conf
contry=US
update_config=1
ctrl_interface=/var/run/wpa_supplicant
network={
  scan=ssid=1
  ssid="<wifi name>"
  psk="<wifi password>"
  key_mgmt=WPA-PSK
}
```
이렇게 한 후에 sd카드를 뽑고 라즈베리 파이에 넣은 후 전원을 넣으면 알아서 와이피이에 연결이 되고 ssh를 사용할 수 있게 된다.  
ssh는 네트워크 상의 다른 컴퓨터에서 원격으로 로그인할 수 있도록 해주는 프로그램이다.  
이제 라즈베리 파이의 ip를 찾아낸다. 특정 ip를 찾아내면 ssh를 이용하여 원격 접속이 가능해 진다.   
```
ssh pi@<ip-address>
//초기 passwd 는 raspberry
```
이제 라즈베리 파이를 구동 시켰으니 k3s를 깔아준다.   
```
sudo apt update
sudo apt upgrade
``` 
file /boot/cmdline.txt add cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory 파일의 끝에 추가.  
그 후에 반드시 reboot를 해주고 k3s를 설치해야 한다.
``` 
// docker 설치
sudo apt install docker.io
// docker 서비스 시작
sudo systemctl start docker
// 부팅시 docker 서비스 시작
sudo systemctl enable docker

//k3s 설치
curl -sfL https://get.k3s.io | sh -s - --docker
#확인
sudo kubectl get nodes
```
나는 containerd가 아닌 docker를 사용하기 위해 옵션을 붙여줬다. 이렇게 되면 master node/ control plane 으로써 만들어진 것을 볼 수 있다. 

이제 Dockerfile을 통해 레이어 형식으로 되어있는 이미지를 만들수 있다(docker build). 
혹은 직접 컨테이너를 docker hub에 있는 최하단 os 이미지를 하나 가져와서 만든 (docker commit)다음에 그 컨테이너 안에서 명령어를 수행하여 세팅을 할 수도 있다. 
컨테이너를 돌리는 것은 docker run 을 사용했다.
필요에 따라 하면 되지만 우리는 led를 연결해서 키는 것을 일단은 원했기 때문에 다음과 같이 진행했다.
먼저 docker hub os image인 raspbian/stretch 를 이용하여 도커 컨테이너를 돌렸고, docker run -it 를 이용하여 컨테이너 안으로 들어가서 led를 구동시킬 파이썬 파일을 만들기 위해 아래와 같은 작업을 해주었다.
```
sudo apt-get update
sudo apt-get install python3
sudo apt-get install python-pip
sudo pip install RPi.GPIO
```
그리고 나서 파이썬 파일을 만들었다.
```
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setup(4, GPIO.OUT)

while(True):
    GPIO.output(4, False)
    time.sleep(2)
    GPIO.output(4,True)
    time.sleep(2)
```
정말 간단하게 무한루프로 led 가 깜빡깜빡 거리게 만든 것이다. 
아무튼 여기에서 문제가 무엇이었냐면 컨테이너는 GPIO 를 사용해서 센서나 이런 것들을 동작시킬 수 있다. 근데 컨테이너를 사용하게 되면 컨테이너에 GPIO 접근 허용에 관한 permission을 줘야 한다는 것이다.
permission은 deployment yaml파일 안에다가 넣어줄 수 있다.

이제 이 이미지를 토대로 yaml파일을 만들어서 deploy를 하려고 했다. 
하지만 당연히 에러가 난다. kubectl create -f .yaml을 통해 만드는 것에는 성공했지만 errImage가 떳다. 
생각해보니 내 컴퓨터에만 이 private 한 이미지를 만들었고, 당연히 쿠버네티스는 docker hub에서 찾을 테니 말이다. 따라서 도커에 로그인을 하고 나만의 이미지를 올려야 했다. 
tag를 붙여주는 방식인데 이렇게 tag를 붙일때는 내 user id를 붙여야한다. 일종의 딱지라고 보면 된다. 이 tag가 없다면 docker의 어떤 유저가 이런 이미지를 만들었는지 알 수 없을 테니 말이다.
```
docker login
// 아이디와 패스워드를 치고
docker tag <내가 만든 이미지> <도커 아이디>/<도커의 내 레포지토리 이름(정하는 것)>
// 이제 태그를 붙였으니 push를 해서 docker hub에 내 이미지를 올린다.
docker push <도커 아이디>/<도커의 내 레포지토리 이름(정하는 것)>
// 이렇게 하면 성공적으로 올라간다.
```

이제 이렇게 만든 이미지를 이용해서 deployment yaml 파일을 만든다.
주의해야 할 점은 라즈베리파이의 GPIO 를 사용해야 하기 때문에 컨테이너에서도 사용하기 위해선 privileged가 있어야 한다. 
yaml 파일의 컨테이너를 정의하는 부분에 securityContext: privileged: true를 작성해주면 이제 컨테이너에서도 라즈베리파이의 GPIO에 접근 할 수 있게 된다.
이렇게 하나의 pod에서 led 작동을 하게 할 수 있고 다른 pod 에서는 또다른 작업을 할 수 있게 되었다. 

이제 남은 과제는 이 많은 pod들을 작동시켜놓고, master node에서 이 pod들을 모니터링 하는 방법을 찾아야 한다.
    





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
마스터 노드로써 돌아가는 것을 확인할 수 있다.  








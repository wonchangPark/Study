# 자바 기초 복습
## 자바의 특징
  이식성이 높음 : 자바 실행 환경(Java Runtime Ennvironment,JRE)이 설치되어 있는 모든 운영체제에서 사용가능.  
  
  객체 지향 언어 : Object Oriented Programming. 부품에 해당하는 객체들을 먼저 만들고, 조립 및 연결해서 전체 프로그램을 완성하는 기법이다.  
  
  막강한 오픈소스 라이브러리 : 검증된 오픈소스 라이브러리를 사용하면 개발 기간을 단축하면서 안전성이 높은 애플리케이션 개발이 가능하다.   
  
  
  
## JVM(java virtual machine)
운영체제는 자바 프로그램을 바로 실행할 수 없다. 자바 프로그램은 완전한 기계어가 아닌 중간 단계의 바이트 코드이기 때문이다. 
따라서 이것을 해석하고 실행할 수 있는 가상의 운영체제가 필요하다. 운영체제별로 프로그램을 실행하고 관리하는 방법이 다르기 때문에 운영체제별로 자바 프로그램을 별도로 개발하는 것보다는 운영체제와 자바 프로그램을 중계하는 
JVM을 두어 자바 프로그램이 여러 운영체제에서 동일한 실행 결과가 나오도록 설계한 것이다. 물론 이렇게 되면 JVM은 각 운영체젱 맞게 설치되어야 한다.   
소스파일인 .java 파일을 작성하고 그것을 컴파일러(javac.exe)로 컴파일하면 확장자가 .class인 바이트 코드 파일이 생성된다. 바이트 코드 파일은 JVM 구동 명령어(java.exe)에 의해 JVM에서 해석되고
해당 운영체제에 맞게 기계어로 번역된다. 이 바이트 코드 파일 하나로 JVM이 있는 어떤 운영체제에서든지 실행이 가능하다.


## JDK(java development kit) 와 JRE(java runtime environment) 차이
JDK에는 프로그램 개발에 필요한 JVM, 라이브러리 API, 컴파일러 등의 개발 도구가 포함.  
JRE에는 프로그램 실행에 필요한 JVM, 라이브러리 API 가 포함.  


## 데이터 타입
1 byte = 8 bit(2^8)    
boolean = 1 byte    
char = 2 byte(양의 정수)   
short = 2 byte   
int = 4 byte   
loge = 8 byte   

정수 연산의 기본 타입은 4바이트 int 형이다. 따라서 byte형이나 short 타입으로 변수를 선언해서 + 연산을 하면 결국 int로 형변환해서 +연산을 하기 때문에 굳이 바꿔봤자 성능 차이는 거의 없다.   

long타입을 사용할 때, 만약 int형을 넘어서는 크기의 수라면 숫자 마지막에 L을 붙여줘야한다.   
long var = 100000000000L; 

실수 타입인 float과 double인 경우 메모리 사용은 int와 long과 같지만, 정수 타입과는 다른 저장 방식 때문에 정수 타입보다 훨씬 더 큰 범위의 값을 저장한다. 지수와 가수 방식으로 저장한다. 가수 m은 0<= m <1 범위의 실수이어야 한다. 예를 들어 1.2345 라고 하면 0.12345 * 10^1 이런 방식으로 저장하는 것이다. 
float: 부호(1bit) + 지수(8bit) + 가수(23bit) = 32 bit
double: 부호(1bit) + 지수(11bit) + 가수(52bit) = 64 bit
실수형의 기본은 double이므로 float을 사용하려면 숫자 뒤에 F를 붙여준다.

자동 타입 변환은 작은 크기를 가지는 타입이 큰 크기를 가지는 타입에 저장될 때 발생한다.   
강제 타입 변환은 캐스팅() 연산자를 통해서 가능하다.   
최대값: 기본 타입.MAX_VALUE 최소값: 기본 타입.MIN_VALUE


## 연산자
삼항 연산자 : (조건식) ? A : B
연산의 경우 오버플로우가 발생하는 지 잘 확인하자. 특히 알고리즘 문제등에서 에러가 나는 경우가 많다.  
int 형을 두 개를 곱했을 때 크기가 너무 커진 경우는 long타입을 사용하자.   
실수의 경우는 정확한 계산이 되지 않기 때문에 정확한 계산을 할 때는 정수형을 이용한다.   
float, double은 0.1을 정확히 표현할 수 없어서 근사치로 표현하기에 정확하지 않다.   
문자열의 비교는 == 이 아닌 equals() 메서드를 이용한다.   

  
## 참조 타입
기본 타입: 정수, 실수, 문자, 논리 리터럴   
참조 타입: 객체의 번지를 참조하는 타입으로 배열, 열거, 클래스, 인터페이스를 말한다.   
변수는 스택 영역에 객체는 힙 영역에 생성된다.   
예를 들어 String name = “독서”라는 변수를 생성했다면, name은 스택영역에 생성이 되지만 “독서”는 힙영역에 생성이 되고 그것의 주소를 name이 값으로 가지고 있는 것이다.   
따라서 name을 호출하면 name은 번지값으로 힙영역에 있는 “독서”에 접근하는 것이다.    
힙영역에서 참조하는 변수나 필드가 없다면 쓰레기로 보고 garbage collector가 제거한다.   
JVM stack 영역은 각 스레드마다 하나씩 존재한다.   

배열에서 String[] names = new String[] { “민수”, “영자”, “지원”} 이런식으로 값을 넣어줄 수도 있다. 하지만 보통은 빈 배열을 만들어 놓고 넣는다.   

프로그램 실행을 위해 main() 메소드가 필요하다. 그리고 그 안의 매개값인 String[] args 도 필요하다.   
java 클래스로 프로그램을 실행하면 JVM은 길이가 0인 String 배열을 먼저 생성하고 main() 메소드를 호출할 때 매개값으로 전달한다. 만약 “java 클래스” 뒤에 공백으로 구분된 문자열 목록을 주고 실행하면, 문자열 목록으로 구성된 String[] 배열이 생성되고 main() 메소드를 호출할 때 매개값으로 전달된다.   
java 클래스 문자열0 문자열1 문자열2   
String[] args = { 문자열0, 문자열1, 문자열2 };   
이 args 가 main() 메소드 호출 시 전달된다. 그러면 main() 메소드는 args 매개 변수를 통해서 커맨드 라인에서 입력된 데이터의 수(배열의 길이)와 입력된 데이터(배열의 항목 값)을 알 게 된다. 이 값들을 args를 이용해서 사용할 수 있게 된다.   
```
public class MainStringArrayArgument {
	public static void main(String[] args){
		if(args.length != 2){
			System.out.println(“프로그램의 사용법”);
			System.out.println(“java MainStringArrayArgument num1 num2”);
			System.exit(0);
		}
		
		String strNum1 = args[0];
		String strNum2 = args[1];

		int num1 = Integer.parseInt(strNum1);
		int num2 = Integer.parseInt(strNum2);

		int result = num1 + num2;
		System.out.println(num1 + “+” + num2 + “=“ + result);
	}
}
```

배열 복사는 System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length); 를 사용한다.   
dest 오브젝트를 src를 토대로 어디부터 어디까지 복사할 것인지를 정할 수 있다.   

향상된 for 문
```
public class AdvancedForExample{
	public static  void main(String[] args){
		int[] scores = { 95, 71, 84, 93, 87 };
		
		int sum = 0;
		for(int score : scores){
			sum = sum + score;
		}
		System.out.println(“점수 총합 = “+ sum);
	}
}
```
scores 배열에서 하나씩 꺼내서 int score에 저장하고 진행한다. 배열에서 가져올 항목이 더이상 없을 때는 for문을 빠져나온다.   


enum은 열거타입이다. 이 때, 열거 상수는 모두 대문자이다.   
public enum Week { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY }   
이렇게 선언을 하고 나면 이 타입을 사용할 수 있다.   
Week today = Week.MONDAY;   
자바는 컴퓨터의 날짜 및 요일, 시간을 사용할 수 있게 Date, Calendar, LocalDateTime 등의 클래스를 제공한다.   

  

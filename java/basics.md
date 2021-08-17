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

## 클래스

객체 지향 프로그래밍의 특징.  
캡슐화 : 객체의 필드, 메소드를 하나로 묶고, 실제 구현 내용을 감추는 것이다. 외부 객체는 객체 내부의 구조를 알지 못하며 객체가 노출해서 제공하는 필드와 메소드만 이용할 수 있다. 캡슐화를 사용하는 이유는 외부의 잘못된 사용으로 인해 객체가 손상되지 않도록 하는 데 있다. 이렇게 노출시킬 것인지 아닐지를 정하는 것으로 접근 제한자(Access Modifier)를 사용한다.   
상속 : 부모(상위) 객체는 자기가 가지고 있는 필드와 메소드를 자식(하위) 객체에게 물려주어 자식 객체가 사용할 수 있게 해준다. 상속은 상위 객체를 재사용해서 하위 객체를 쉽고 빨리 설계할 수 있도록 도와주고, 반복된 코드의 중복을 줄여준다. 이렇게 함으로써 유지 보수 시간을 최소화시켜주기도 한다.   
다형성(Polymorphism) : 다형성은 같은 타입이지만 실행 결과가 다양한 객체를 이용할 수 있는 성질을 말한다. 즉, 하나의 타입에 여러 객체를 대입함으로써 다양한 기능을 이용할 수 있다. 다형성의 효과로 객체는 부품화가 가능하다. 예를 들어 자동차를 설계할 때 타이어 인터페이스 타입을 적용했다면 이 인터페이스를 구현한 실제 타이어들은 어떤 것이든 상관없이 장착(대입)이 가능하다.   

클래스의 구성 멤버는 field, constructor, method가 있다. 생성자는 당연히 객체 생성 시 초기화 하는 역할을 담당한다.   
new 연산자와 같이 사용되어 클래스로부터 객체를 생성할 때 호출되어 객체의 초기화를 담당한다.   
생성자가 성공적으로 실행되면 힙 영역에 객체가 생성되고 객체의 주소가 리턴된다.   
생성자 오버로딩을 통해 다양한 데이터를 이용해서 객체를 초기화 한다.   
```
public class Car {

	//field
	String company = “현대자동차”;
	String model;
	String color;
	int maxSpeed;

	// contructor
	Car(){}

	Car(String model) {
		this(model, “은색”, 250);
	}

	Car(String model, String color) {
		this(model, color, 250);
	}

	Car(String model, String color, int maxSpeed){
		this.model = model;
		this.color = color;
		this.maxSpeed = maxSpeed;
	}
}
```
메소드의 경우 매개 변수의 수를 모르는 경우가 있다. 이럴 경우는 배열을 넘겨준다.   
int sum1(int[] values) {}   
혹은 배열말고 그냥 수를 리스트 형태로 넘겨도 된다.   
int sum2(int … values) {}   
이렇게 정의를 하면 sum2(1,2,3,4,5); 이런 식으로 수를 어떻게 줘도 상관없다.   
그리고 메소드 또한 오버로딩을 이용하여 매개 변수의 타입, 개수, 순서 를 다르게 함으로써 같은 이름의 메소드를 만들 수 있다.   

## 정적 멤버와 static
정적 멤버는 클래스에 고정된 멤버로서 객체를 생성하지 않고 사용할 수 있는 필드와 메소드를 말한다. 정적 멤버는 객체에 소속된 멤버가 아니라 클래스에 소속된 멤버이다.   
static을 하는 것에 대한 기준은 객체마다 가지고 있어야 할 데이터라면 인스턴스 필드로 선언하고, 공용적인 데이터라면 정적 필드로 선언하는 것이 좋다.   
예를 들어 Calculator 클래스에서 파이 변수는 객체마다 가지고 있을 필요가 없으므로 정적 필드로 선언하는 것이 좋다.   
메소드의 경우의 기준은 인스턴스 필드를 이용해서 실행해야 하면 인스턴스 메소드로 선언하고 아니라면 정적 메소드가 좋다.   
예는 Calculator 클래스의 덧셈, 뺄셈 기능은 인스턴스 필드를 이용하기 보다는 외부에서 주어진 매개값들을 가지고 수행하므로 정적 메소드로 선언하는 것이 좋다. 정적 멤버의 경우는 인스턴스가 없으므로 클래스.메소드 이런식으로 사용한다.   
정적 필드는 생성자가 없으므로 초기화 작업은 정적 블록인 static{} 에서 한다.   
```
public class Television {
	static String company = “Samsung”;
	static String model = “LCD”;
	static String info;

	static {
		info = company + “-“ + model;
	}
}
```
당연한 거지만 정적 블록이나 정적 메소드 안에서는 정적 필드가 인스턴스 필드나 인스턴스 메소드는 들어갈 수가 없다. 그래서 static 인 main 메소드도 main 메소드 밖의 선언된 필드가 static이 아니면 에러가 나는 이유이다.    
```
public class Car {
	int speed;

	public static void main(String[] args){
		Car myCar = new Car(); // 클래스로 인스턴스를 만들어주고 인스턴스 필드인 speed를 사용해줘야함
		myCar.speed = 60;
	}
}
```

## 싱글톤(Singleton)
전체 프로그램에서 단 하나의 객체만 만드는 경우가 있다. 싱글톤을 만드려면 클래스 외부에서 new 연산자로 생성자를 호출할 수 없도록 막아야 한다.   
생성자를 외부에서 호출할 수 없도록 하려면 생성자 앞에 private 접근 제한 자를 붙여준다. private 접근 제한자는 모든 외부 클래스에서 접근하지 못하게 하는 것이다.   
그리고 자신의 타입인 정적 필드를 하나 선언하고 자신의 객체를 생성해 초기화 한다.   
```
public class Singleton {
	private static Singleton singleton = new Singleton();
	// static 선언으로 인스턴스없이 만들수 있게 하고, private으로 인해 내부에서만 사용가능하게 해준다.

	private Singleton(){}

	static Singleton getInstance() {
		return singleton;
	}
}
```
Singleton obj1 = Singleton.getInstance();   
이런식으로 사용해주면 된다. 아무리 많은 객체를 만들어도 오직 하나의 객체만이 사용된다.   

## final 필드와 상수
final 필드는 초기값이 저장되면 최종적인 값이 되어서 프로그램 실행 도중 수정할 수 없다는 것이다.   
final 필드의 초기값을 주는 방법은 딱 두가지 이다. 필드 선언 시에 주는 방법, 생성자에서 주는 방법.  
예제를 들면 주민등록번호 필드는 한 번 값이 저장되면 변경할 수 없도록 final 필드로 선언한다.    
하지만 주민등록번호는 Person 객체가 생성될 때 부여되므로 Person 클래스 설계 시 초기값을 미리 줄 수 없다.    
그래서 생성자 매개값으로 주민등록번호를 받아서 초기값으로 지정해준다.   
반면 nation은 항상 고정된 값을 갖기 때문에 필드 선언 시 초기값으로 “Korea”를 주었다.   
```
public class Person {
	final String nation = “Korea”;
	final String ssn;
	String name;

	public Person(String ssn, String name){
		this.ssn = ssn;
		this.name = name;
	}
}
```
 ## 상수 (static final)
말 그대로 정적이면서 최종적인 값이다. 인스턴스가 없으면서 최종적인 값이니 그냥 고정된 상수이다.   

## 어노테이션 (annotation)
어노테이션은 메타데이터라고 볼 수 있다. metadata 란 애플리케이션이 처리해야 할 데이터가 아니라 컴파일 과정과 실행 과정에서 코드를 어떻게 컴파일하고 처리할 지를 알려주는 정보이다.   
-> 컴파일러에게 코드 문법 에러를 체크하도록 정보를 제공.  
-> 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공.  
-> 실행 시 특정 기능을 실행하도록 정보를 제공.  


## 상속

상속을 해도 부모 클래스의 모든 필드와 메소드들을 물려받는 것은 아니다. 부모 클래스의 private 접근 제한을 갖는 필드와 메소드는 제외된다.   
상속은 extends 를 사용해서 부모 클래스를 정한다.   
자바는 다중 상속이 안된다.   
지식 객체를 생성하면, 부모 객체가 먼저 생성되고 자식 객체가 그 다음에 생성된다.   
super()를 통해 부모 생성자를 호출한다.   
final 은 수정될 수가 없다는 것이므로 상속을 할 수 없다.   
public final class 클래스{ …} 이렇게 클래스를 정의하면. 상속이 불가능하게 된다.   

## 메소드 재정의
부모  클래스의 어떤 메소드는 자식 클래스가 사용하기에 적합하지 않을 수도 있다. 이 경우 상속된 일부 메소드는 자식 클래스에서 다시 수정해서 사용해야 한다.   
이 때, overriding 기능을 사용한다.   
```
public class Calculator {
	double areaCircle(double r) {
		System.out.println(“Calculator 객체의 areaCircle() 실행”);
		return 3.14159 * r * r;
	}
}

public class Computer extends Calculator {
	@Override
	double areaCircle(double r){
		System.out.println(“Computer 객체의 areaCircle() 실행”);
		return Math.PI * r * r;
	}
}
```
## 부모 메소드 호출(super)
super.부모메소드(); 이런 식으로 자식 클래스에서 부모 클래스의 메소드를 호출할 수 있다.

## 다형성
자식은 부모의 특징과 기능을 상속받기 때문에 부모와 동일하게 취급될 수 있다. 
```
class Animal{…}   
class Cat extends Animal{…}   
Cat cat = new Cat();   
Animal animal = cat;   
```
이렇게 하면 cat과 animal은 타입만 다를뿐 동일한 Cat 객체를 참조한다.

다형성을 하용하는 이유는 동일한 타입을 사용하지만 다양한 결과가 나오는 성질 때문에 그렇다.
``` 
class Car {
	//field
	Tire frontLeftTire = new Tire();
	Tire frontRightTire = new Tire();
	Tire backLeftTire = new Tire();
	Tire backRightTire = new Tire();

	//method
	void run(){
		frontLeftTire.roll();
		frontRightTire.roll();
		backLeftTire.roll();
		backRightTire.roll();
	}
}
 
Car myCar = new Car();
myCar.frontRightTire = new HankookTire();
myCar.backLeftTire = new KumhoTire();
myCar.run();
```
Car 클래스에는 4개의 Tire 필드가 있지만  그 중 두개의 타이어를 Tire의 자식 클래스인 HankookTire 와 KumhoTire로 교체했다.   
이것이 가능한 것이 다형성이다. 그리고 Tire의 roll 메소드를 재정의 했기 때문에 run()을 호출해도 에러가 나지 않는다. 이렇게 부품화가 가능해지는 것이다.   

instanceof 연산자를 사용하면 객체가 어떤 클래스의 인스턴지인지 확인할 수 있다.   
boolean result = 객체 instanceof 타입   

## 추상 클래스
추상 클래스는 인터페이스와 비슷하다. 실체 클래스들의 공통되는 필드와 메소드를 따로 선언한 클래스가 추상 클래스이다. 추상클래스는 객체로 생성할 수가 없다.   
오직 부모 클래스로만 사용된다. 추상 클래스를 사용하는 이유는 2가지 이다.   
1. 실체 클래스들의 공통된 필드와 메소드의 이름을 통일할 목적 : 실체 클래스를 설계하는 사람이 여러 사람일 경우, 동일한 데이터와 기능임에도 불구하고 이름이 다를 수 있다. 예를 들어, 전원을 켜다라는 메소드를 Telephone에서는 turnOn(), SmartPhone 에서는 powerOn() 이라고 설계할 수 있다. 따라서 Telephone 과 SmartPhone은 Phone을 상속함으로써 필드와 메소드 이름을 통일 시킬 수 있다.
2. 실체 클래스를 작성할 때 시간을 절약

추상 클래스는 abstract를 사용해서 선언한다. 인터페이스의 경우는 필드는 오직 상수필드만 사용할 수 있지만 추상 클래스는 필드를 생성할 수 있다.   
 
## BufferedReader 와 BufferedWriter
BufferedReader 와 BufferedWriter는 말 그대로 입력과 출력에 버퍼를 사용하는 것이다.   
일반적인 input은 키보드의 입력이 키를 누르는 즉시 바로 프로그램에 전달된다. 하지만 버퍼를 사용하면 키보드의 입력이 있을 때마다 한 문자씩 버퍼로 전송한다.   
버퍼가 가득 차거나 개행 문자가 나타나면 버퍼의 내용을 한 번에 프로그램에 전달한다.   
버퍼를 거쳐 출력하는 것보다 즉시 출력이 더 빠르다고 생각할 수 있으나 하드 디스크는 속도가 느리고 외부 장치(키보드, 모니터)와 데이터 입출력도 시간이 오래 걸린다. 따라서 중간에 버퍼를 두어 한 번에 묶어 보내는 것이 더 빠르다.   

Scanner 는 띄어쓰기와 개행문자를 경계로 하여 입력 값을 인식한다. 따라서 원하는 타입의 입력을 받을 수 있다.   
int a = scanner.nextInt() 이렇게 하면 바로 int 값으로 받아올 수 있다.   
하지만 버퍼는 무조건 String 으로 고정되기 때문에 입력받은 데이터를 우리가 다시 타입을 바꿔줘야 한다.   
Scanner 가 확실히 더 편하지만 버퍼 사이즈가 1024 char이기 때문에 많은 입력을 필요로 할 경우에는 성능이 굉장히 안좋아진다.   

BufferedReader 는 개행문자만 경계로 인식한다. 버퍼 사이즈가 8192 char(16384byte) 이기 때문에 입력이 많을 때 Scanner 보다 훨씬 좋다.  
멀티 스레드 환경에서는 BufferedReader 가 동기화 되기 때문에 안전하고, Scanner는 동기화가 되지 않는다.   

BufferedWriter 는 많은 출력이 필요할 때 사용한다. 보통은 System.out.println()을 사용하지만 많은 출력일 땐 이렇게 버퍼를 사용하는 것이 좋다.   
```
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
String str = “abcdef”;
bw.write(str);
bw.newLine(); // 개행
bw.flush();
bw.close();
```
BufferedWriter 의 경우 버퍼를 잡아 놓았기 때문에 반드시 사용한 후에 flush() 를 사용하여 남아있는 데이터를 모두 출력하고 close() 를 통해 닫아주어야 한다.   
close() 를 하고 나면 출력 스트림을 아예 닫아버리기 때문에 모두 출력하고 나서 close() 를 사용해 주자.   

String은 불변성을 갖는다. concat이나 +연산자를 이용해도 실제의 String 값을 버리고 새로운 값으로 재할당하는 것이다.   
이 불변성의 장점은 변하지 않는 문자열을 자주 읽어들이는 경우 그 주소로 찾아가 읽기만 하면 되기 때문에 아주 좋다.   
하지만 문자열에 변화가 일어나는 경우에는 힙, 메모리에 많은 garbage가 생성되고, 이는 힙 메모리 부족으로 되어 프로그램 성능에 영향을 미친다.   
이를 해결하기 위해 나온 것이 StringBuffer/StringBuilder이다. 이것들은 가변성을 가져서 append(), delete() 로 언제든 바꿀 수 있다.   
StringBuffer 와 StringBuilder의 차이점은 StringBuffer 는 동기화를 지원하여 멀티 스레드 환경에서 안전하다.   
하지만 StringBuilder 는 동기화를 지원하지 않는다. 만약 단일 스레드라면 StringBuffer 보다 성능이 뛰어나다.   

## 인터페이스
모든 메소드가 추상 메소드이다. 추상 클래스보다 한 단계 더 추상적인 것이다.   
따라서 인터페이스를 만들때 abstract를 붙이지 않아도 추상메소드라는 것을 알아야 한다.   
그리고 추상클래스는 단일 상속인데, 인터페이스는 다중 상속이 된다.   
추상 클래스의 목적은 상속을 받아서 기능을 확장시키는 것.  
인터페이스의 목적은 구현하는 모든 클래스에 대해 특정한 메소드가 반드시 존재하도록 강제하는 역할.  

인터페이스는 객체의 교환성을 높여주기 때문에 다형성을 구현하는 매우 중요한 역할을 한다.   
다형성은 하나의 타입에 대입되는 객체에 따라서 실행 결과가 다양한 형태로 나오는 성질을 말한다.   
인터페이스는 개발 코드와 객체가 서로 통신하는 접점 역할을 한다. 개발 코드가 인터페이스의 메소드를 호출하면 인터페이스는 객체의 메소드를 호출시킨다.   
따라서 개발 코드는 객체의 내부 구조를 알 필요가 없고 인터페이스의 메소드만 알고 있으면 된다.   
중간에 인터페이스를 두는 이유는 개발 코드를 수정하지 않고, 사용하는 객체를 변경할 수 있도록 하기 위해서이다.   
인터페이스는 하나의 객체가 아니라 여러 객체들과 사용이 가능하므로 어떤 객체를 사용하느냐에 따라서 실행 내용과 리턴값이 다를 수 있다.   
인터페이스는 오직 상수와 메소드(abstract method, default method, static method)만을 구성 멤버로 가진다.   

## 익명 구현 객체
구현 클래스를 만들어 사용하는 것이 일반적이고, 클래스를 재사용할 수 있기 때문에 편리하지만, 일회성의 구현 객체를 만들기 위해 소스 파일을 만들고 클래스를 선언하는 것은 비효율적이다. 따라서 소스 파일 없이 구현 객체를 만드는 것이 익명 구현 객체이다.   
```
public class RemoteControlExample {
	public static void main(String[] args){
		RemoteControl rc = new RemoteControl(){
			public void turnOn(){ /*실행문*/}
			public void turnOff(){ /*실행문*/}
			public void setVolume(int volume){ /*실행문*/}
		};
	}
}
```

## 다중 인터페이스 구현 클래스
인터페이스A 와 인터페이스B 가 객체의 메소드를 호출할 수 있으려면 객체는 이 두 인터페이스를 모두 구현해야 한다.   

## 인터페이스 예제

먼저 인터페이스를 만들어준다.   
```
public interface RemoteControl{
	// 상수
	int MAX_VOLUME = 10;
	int MIN_VOLUME = 0;
	
	// 추상 메소드
	void turnOn();
	void turnOff();
	void setVolume(int volume);

	// 디폴트 메소드
	default void setMute(boolean mute){
		if(mute){
			System.out.println(“무음 처리합니다.”);
		}
	}

	// 정적 메소드
	static void changeBattery(){
		System.out.println(“건전지를 교환합니다.”);
	}
}
```

인터페이스를 상속받은 클래스를 만들어준다.   
```
public class Television implements RemoteControl{
	// 필드
	private int volume;

	// turnOn() 추상 메소드의 실체 메소드
	public void turnOn(){
		System.out.println(“TV를 켭니다.”);
	}
	// turnOff() 추상 메소드의 실체 메소드
	public void turnOff(){
		System.out.println(“TV를 끕니다.”);
	}
	// setVolume() 추상 메소드의 실체 메소드
	public void setVolume(int volume){
		if(volume>RemoteControl.MAX_VOLUME){
			this.volume = RemoteControl.MAX_VOLUME;
		} else if { … }
	}
}
```

만약 위처럼 구체적으로 추상메소드들을 구현하지 않고 하나라도 빼먹으면 에러가 뜨며 추상 클래스로 만들던가 모든 추상메소드를 구현하던가 해야 한다.   

이제 인터페이스 사용   
```
public class RemoteControlExample{
	public static void main(String[] args){
		RemoteControl rc;
		rc = new Television();
		rc.turnOn(); // television의 turnOn
		rc = new Audio();
		rc.turnOn(); // audio의 turnOn
	}
}
```
이런식으로 인터페이스에 구현 클래스를 넣으면 언제든 객체를 교환할 수 있게 되고 다형성을 구현할 수 있게 된다.   

디폴트 메소드는 인터페이스에 선언되었지만, 인터페이스에서 바로 사용할 수 없다. 추상 메소드가 아닌 인스턴스 메소드이므로 구현 객체가 있어야 사용할 수 있다.   
쉽게 생각하면, 인터페이스의 모든 구현 객체가 가지고 있는 기본 메소드이다.   
하지만 특정 구현 객체는 내용 수정이 필요할 수도 있다.   
그러면 디폴트 메소드를 오버라이딩해서 자신에게 맞게 수정하면 된다.   

정적 메소드는 인터페이스로 바로 호출이 가능하다.   

인터페이스를 배열로 구현 객체 관리가 가능하다.   
위의 예제인 remoteControl을 보면 2개의 remoteControl을 television과 audio로 각각 선언했지만 배열을 사용해서 선언도 가능하다.   
```
RemoteControl[] controls = {
	new Television(),
	new Audio()
};
```
이렇게 하면 향상된 for 문을 사용해서 할 수도 있다.   
```
for(RemoteControl control : controls){
	control.turnOn();
}
```

우리는 인터페이스를 사용해서 다형성을 구현했다. 즉, 인터페이스 에다가 구현객체를 넣어서 그 인터페이스를  사용했다.   
이렇게 사용하면 자동변환이 빈번하게 일어나게 되는데, 어떤 구현 객체가 변환되어 있는지 알 수 없는 상태에서 무작정 변환을 할 경우 ClassCastException이 일어날 수 있다. 따라서 어떤 구현 객체가 인터페이스 타입으로 변환되었는지 확인하기 위해 instanceof 연산자를 사용한다.   
```
if( vehicle instanceof Bus){
	Bus bus = (Bus) vehicle;
}
```

Vehicle 이라는 인터페이스를 만들고 메소드로 run()을 만들었다고 하자.   
그리고 이 Vehicle을 상속하는 Taxi와 Bus를 만들었다고 하자. 이 Taxi와 Bus에는 checkFare() 메소드가 있다.   
이런 경우엔 Vehicle 이라는 인터페이스에 구현 객체인 Bus를 넣었다고 하면, Bus 에만 있는 checkFare()은 사용할 수가 없고, 강제 변환으로 Bus로 만들어준 뒤에 사용할 수 있다. 이럴때에도 instanceof를 사용하여 Vehicle이 Bus 구현 객체로 변환이 되어있는 지를 확인한 다음에 사용하도록 한다.   

인터페이스는 또다른 인터페이스를 상속할 수 있으며, 다중 상속이 가능하다.   
```
public interface 하위 인터페이스 extends 상위인터페이스1, 상위인터페이스2{…}
```

하위 인터페이스를 구현하는 클래스는 하위 인터페이스의 메소드뿐만 아니라 상위 인터페이스의 모든 추상 메소드에 대한 실체 메소드를 구현 해야 한다.   
그렇기 때문에 다음과 같이 하위 및 상위 인터페이스 타입으로 변환이 가능하다.   
하위인터페이스 변수 = new 구현클래스();   
상위인터페이스1 변수 = new 구현클래스();   
상위인터페이스2 = new 구현클래스();   

디폴트 메소드는 도대체 왜 만든 것일까?   
유지보수에 있다.   
기존에 MyInterface 라는 인터페이스와 이를 구현한 MyClassA 라는 클래스가 있다고 하자.    
나중에 MyInterface에 기능을 추가해야할 필요성이 생겼다. 그래서 MyInterface에 추상 메소드를 추가했는데, 그렇게 되면 MyClassA에서 문제가 생긴다.   
추가된 추상 메소드에 대한 실체 메소드가 MyClassA에 없기 때문이다. 그래서 이 디폴트 메소드를 사용한다.   
디폴트 메소드는 추상 메소드가 아니기 때문에 구현 클래스에서 실체 메소드를 작성할 필요가 없다.   
따라서 MyClassA 는 아무런 문제 없이 계속 사용이 가능하다.   

## 중첩 클래스와 중첩 인터페이스
중첩 클래스(nested class)란 클래스 내부에 선언한 클래스이다.   
여러 클래스와 관계를 맺는 경우가 아닌 특정 클래스랑만 관계를 맺을 경우에는 이렇게 중첩 클래스를 사용하는 것이 좋다.   
중첩 클래스의 장점은 두 클래스의 멤버들은 서로 쉽게 접근할 수 있다는 것과 불필요한 관계 클래스를 감춤으로써 코드의 복잡성을 줄일 수 있다.   

## 중첩 클래스
클래스의 멤버로서 선언되면 멤버 클래스이고 메소드 내부에서 선언되는 중첩 클래스는 로컬 클래스라고 한다.   
당연히 로컬 클래스는 해당 메소드가 종료되면 없어진다.   

## 인스턴스 멤버 클래스
static 을 사용하지 않은 클래스이다. 이 경우, 인스턴스 필드와 메소드만 선언이 가능하고 정적 필드와 메소드는 선언할 수 없다.   
```
class A {
	// 인스턴스 멤버 클래스
	class B {
		B(){} // 생성자
		int field1; // 인스턴스 필드
		// static int field2; // 정적 X
		void method1(){} // 인스턴스 메소드
		// static void method2(){} // 정적 메소드 X
	}
}

// B 클래스를 사용하는 법
A a = new A();
A.B b = a.new B();
b.field1 = 3;
b.method1();
```
## 정적 멤버 클래스
static 을 사용한 클래스이다. 모든 종류의 필드와 메소드를 선언할 수 있다.   
A 클래스 외부에서 정적 멤버 클래스 C의 객체를 생성하기 위해서는 A 객체를 생성할 필요없이 바로 C 객체를 생성하면 된다.   
```
class A {
	// static 멤버 클래스
	class B {
		B(){} // 생성자
		int field1; // 인스턴스 필드
		static int field2; // 정적 필드
		void method1(){} // 인스턴스 메소드
		static void method2(){} // 정적 메소드 
	}
}

A.C c = new A.C();
c.field1 = 3; // 인스턴스 필드 사용
c.method1(); // 인스턴스 메소드 사용
A.C.field2; // 정적 필드 사용
A.C.method2(); // 정적 메소드 사용 
```


## 로컬 클래스
메소드 내에서도 선언이 가능하다. 메소드 내에서만 사용하므로 접근 제한자나 static을 붙일 수 없다.   
주로 비동기 처리를 위한 스레드 객체를 만들 때 사용한다.   
로컬 클래스에서 사용된 매개 변수와 로컬 변수는 모드 final 특성을 갖는다.   
```
void method(){
	//로컬 메소드
	class D{
		D(){}
		int field;
		void method1(){}
	}
	D d = new D();
	d.field1 = 3;
	d.method1();
}
```

## 중첩 인터페이스
주로 이벤트 처리 목적으로 이용된다.   

중첩 인터페이스
```
public class Button{
	OnClickListener listener; // 인터페이스 타입의 필드
	
	void setOnClickListener(OnClickListener listener){
		this.listener = listener;
	}

	void touch(){
		listener.onClick();
	}
	
	interface OnClickLister{
		void onClick();
	}
}
```
구현 클래스
```
public class CallListener implements Button.OnClickListener{
	@Override
	public void onClick(){
		System.out.println(“전화를 겁니다.”);
	}
}
```
구현 클래스
```
public class MessageListener implements Button.OnClickListener{
	@Override
	public void onClick(){
		System.out.println(“메세지를 보냅니다.”);
	}
}
```
버튼 이벤트 처리
```
public class ButtonExample{
	public static void main(String[] args){
		Button btn = new Button();

		btn.setOnClickListener(new CallListener());
		btn.touch(); // 전화

		btn.setOnClickListener(new MessageListener());
		btn.touch(); // 메세지
	}
}
```

## 익명 객체
이벤트 처리나 스레드 객체를 간편하게 사용할 목적으로 쓴다. 말 그대로 이름이 없어서 익명 객체라고 한다.   
```
class A {
	// 익명 자식 객체 생성
	Parent field = new Parent(){
		int childField;
		void childMethod(){}
		@Override
		void parentMethod(){}
	}; // 하나의 실행문이기 때문에 세미콜론을 붙여줘야 한다.

	void method(){
		//field.childField = 3; // 접근 안됨. Parent 타입으로 대입되기 때문에 자식 객체 내부의 필드나 				 	    메소드는 오직 자식 객체 안에서만 사용이 가능하다.
		//field.childMethod(); // 위의 이유로 이것 또한 안됨.
		field.parentMethod(); // 가능
} 
```

인터페이스에서도 사용이 가능하다. 인터페이스를 상속한 구현 객체를 만들 수 있다.   

위에서 사용한 Button 클래스를 사용하겠다.   
```
public class Window {
	Button button1 = new Button();
	Button button2 = new Button();
	
	// 필드 초기값으로 대입
	Button.OnClickListener listener = new Button.OnclickListener{
		@Override
		public void onClick(){
			System.out.println(“전화를 겁니다.”);
		}
	};

	window() {
		button1.setOnClickListener(listener);
		button2.setOnClickListener( new Button.OnClickListener(){
			@Override
			public void onClick(){
				System.out.println(“메세지를 보냅니다.”);
			}
		};
	}
}
```
실행 클래스
```
public class Main{
	public static void main(String[] args){
		Window w = new Window();
		w.button1.touch();
		w.button2.touch();
	}
}
```

메소드 내에서 생성된 익명 객체는 메소드 실행이 끝나도 힙 메모리에 존재해서 계속 사용할 수 있다.   
매개 변수나 로컬 변수는 메소드 실행이 끝나면 스택 메모리에서 사라지기 때문에 익명 객체에서 사용할 수 없게 되므로 문제가 발생한다.   
따라서 익명 객체 내부에서 메소드의 매개 변수나 로컬 변수를 사용할 경우, 이 변수들은 final 특성을 가져야 한다.   
익명 객체에서 사용된 매개 변수와 로컬 변수는 모두 final 특성을 갖는다고 생각하면 된다.   

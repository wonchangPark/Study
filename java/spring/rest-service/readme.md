# Building a RESTful Web Serivce
“Hello, World” RESTful web service를 만들어 보자.    
HTTP GET 요청을 받는 service를 만들 것이다.   
http://localhost:8080/greeting   
get 요청을 받으면 200 OK 와 JSON 형태로 답을 해줄 것이다.   
{"id":1,"content":"Hello, World!”}   

//참고로 인텔리제이가 무한 indexing 중일 땐 file-> invalidate caches 를 통해 캐시를 삭제하면 해결된다.

여기에서 name 파라미터를 쿼리에 넣어서 보내면 그것에 맞게 만든 답을 줄 것이다.   
요청 : http://localhost:8080/greeting?name=User   
name 파라미터가 World를 덮어쓸것이다.
응답 : {“id":1,"content":"Hello, User!”}

## create a resource representation class
먼저 resource representation class를 만들어야 한다.   
Greeting class를 만듬으로써 id와 content에 대한 필드와 생성자를 제공한다.   
쉽게 말해 자원을 나타내는 클래스를 만든다는 것이다.   
```
package com.example.restservice;

public class Greeting {

	private final long id;
	private final String content;

	public Greeting(long id, String content) {
		this.id = id;
		this.content = content;
	}

	public long getId() {
		return id;
	}

	public String getContent() {
		return content;
	}
}
```
## create a resource controller
RESTful web services를 만드는 것에 대한 스프링의 접근으로써, HTTP request는 controller에 의해 처리된다.   
이 components는 @RestController 어노테이션으로 식별할 수 있다. 즉, @RestController 어노테이션을 사용하면 HTTP request를 처리하는 controller라는 것 이다.   
여기에서 만들 controller는 /greeting의 GET 요청을 처리한다. 그리고 Greeting class의 새로운 인스턴스를 반환한다.   
```
package com.example.restservice;

import java.util.concurrent.atomic.AtomicLong;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

	private static final String template = "Hello, %s!";
	private final AtomicLong counter = new AtomicLong();

	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
}
```

@GetMapping 어노테이션은 /greeting에 오는 HTTP GET 요청을 greeting() 메서드로 매핑시킨다.   
(post를 할 경우는 @PostMapping, 그냥 다 포함한 것을 사용하려면 @RequestMapping(method=GET) 이런 식으로 사용한다.)   

@RequesetParam은 query string parameter인 name을 greeting() 메서드에 있는 name parameter에 연결(binding)해준다. 만약 name 파라미터가 없다면 기본(defaultValue)으로 World를 사용한다.   
그리고선 새로운 Greeting 오브젝트를 받은 id와 content를 기반으로 만들어서 반환한다.   
예전의 MVC 컨트롤러와 RESTful web service 컨트롤러의 주요한 차이는 HTTP response body가 만들어진다는 것이다.   
서버쪽의 greeting data를 HTML로 표현하는 view 기술에 의존하기 보단 RESTful web serice controller가 Greeting 이라는 오브젝트를 반환한다.   
이 오브젝트 데이터는 HTTP response로 JSON 형태로 바로 쓰여진다.   
이 코드에서는 @RestController 어노테이션을 사용해서 이 어노테이션을 사용하고 있는 클래스가 view 대신에 오브젝트를 반환하는 컨트롤러라는 것을 말하는 것이다. @Controller 와 @ResponseBody를 합친 것과 같다.   
Greeting 오브젝트는 JSON 형태로 변환되어야 하지만 Spring의 HTTP message converter로 인해서 우리가 할 일은 없다.   

@SpringBootApplication 은 아래의 모든 것들을 합친 것이다.
> @Configuration : tags the class as a source of bean definitions for the application context. application context의 빈 등록과 같은 것이다.   
> @EnableAutoConfiguration : Spring Boot에게 classpath setting, other beans, 다양한 property settings에 의존하여 빈들을 등록하라고 하는 것이다. 예를 들어, spring-webmvc가 classpath에 있다면 이 어노테이션은 어플리케이션을 웹 어플리케이션으로 하고, DispatcherServlet 같은 주 기능을 하는 것들을 활성화 시킨다.   
> @ComponentScan : Spring에게 다른 components, configurations, services를 com/example 패키지에서 controller들을 찾아보라고 하는 것이다.   

main()은 Spring Boot의 SpringApplication.run()을 사용한다. 
web.xml 파일은 필요없다.

이제 이 완성된 프로젝트를 구동하면 되는 데, ide를 사용해도 되고, 이동이 편리하고, 배포가 편한 모든 필요한 dependencies, classes, resources가 들어있는 JAR 파일로 만들어도 된다.
Gradle 일 경우, ./gradlew bootRun 을 통해 어플리케이션을 구동할 수 있다. 혹은 Jar file을 만들고 run해도 된다.
java -jar build/libs/gs-rest-service-0.1.0.jar

Maven 일 경우, ./mvnw spring-boot:run 을 통해 할 수 있고, Jar file의 경우는 ./mvnw clean package 를 하고 
java -jar target/gs-rest-service-0.1.0.jar 으로 하면 된다.

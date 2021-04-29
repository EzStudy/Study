# Swagger?

스웨거(Swagger)는 개발자가 REST 웹 서비스를 설계, 빌드, 문서화, 소비하는 일을 도와주는 대형 도구 생태걔의 지원을 받는 오픈 소스 스프트웨어 프레임워크입니다. 현재 진행하고 있는 프로젝트에 대해서 유지보수를 진행하거나 API를 만들게 될때 API서버가 어떤 스펙을 가지고 있는지 파악해야 한다. Swagger는 API의 명세와 문서를 대신 작성해 주는 아주 착한 친구 입니다.

![swagger](https://static1.smartbear.co/swagger/media/assets/images/swagger_logo.svg)

[Swagger]: https://swagger.io/	"Swagger"

스웨거의 공식 사이트입니다. 저 자세한 정보르 알고싶다면 위 링크에 접속해서 알아보시면 될거에요.

여러가지 환경에서 Swagger를 사용가능한데 이 글에서는 Spring Boot + Swagger 세팅을 해볼께요.



# Spring Boot + Swagger

SpringBoot 스펙

* JDK 8
* Spring Boot 2.4.5
* Mave

처음에 Dependency를 추가해 줍니다.

```xml
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.9.2</version>
</dependency>
```

* springfox-swagger2는 Swagger를 사용하기 위한 필수 라이브러리입니다.
* springfox-swagger-ui는 API스펙 명세화 기능 사용등 여러가지 기능을 UI로 사용하기 위해서 필요한 라이브러리 입니다.

관련 라이브러리가 다 다운되면 Swagger관련되 설정을 하기 위한 설정 클래스 SwaggerConfig를 만들어줍니다.

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
	
	@Bean
	public Docket api() {
		return new Docket(DocumentationType.SWAGGER_2)
				.select()
				.apis(RequestHandlerSelectors.any())//Swagger API 문서로 만들기 원하는 basePackage 경로
				.paths(PathSelectors.ant("/*/**"))	//URL 경로를 지정하여 해당 URL에 해당하는 요청만 SWAGGER로 만듦
				.build();
	}
}
```

* apis() 는 Swagger API로 문서로 만들기 원하는 basePackage 경로를 적으면 됩니다. 저는 모든 곳을 문서화 하고싶기 때문에 `RequestHandlerSelector.any()` 로 지정해주었습니다.
* path() 는 그 안의 경로의 하위만 Swagger로 만들겠다는 이야기 입니다.
  * `PathSelectors.ant("/api/**")`로 적게되면 /api 하위의 모든 API를 Swagger에서 볼 수 있도록 설정하겠다는 뜻입니다.

* 추가적인 Swagger 환경설정을 할수 있습니다. 다만 이 글은 진짜 간단하게 연동하기 위함으로 생략하겠습니다.
  * `consumes()` ` produces() ` `apiInfo()`등의 함수를 이용해서 여러가지 설정을 할 수 있습니다.



그 다음 Controller를 작성해 줍니다.

```java
@Controller
public class HomeController {
	
	@RequestMapping("/")
	public String index() {
		return "login";
	}
	
	@GetMapping("/login")
	public String login(HttpServletResponse res,
			@RequestParam(value="login_id",required=true) String login_id,
			@RequestParam(value = "password", required=true) String password) {
		
		return "";
	}
}
```


로그 설정하기

@Slf4j 
롬복 사용 

로그 레벨 설정
LEVEL: TRACE > DEBUG > INFO > WARN > ERROR

application.properties
여기서 설정한다.

만약 전체 로그 레벨을 설정하고 싶다면

logging.level.root=info

패키지명에 따라

밑에는 hello.springmvc 하위에 모두 로그를 작성.
logging.level.hello.springmvc=debug

## @RestController

@Controller 는 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 랜더링 다. 
@RestController 는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다.

## @RequestMapping

대부분의 속성을 배열[] 로 제공하므로 다중 설정이 가능하다. {"/hello-basic", "/hello-go"}

## @PathVariable(경로 변수)

사용 시점

URL 경로를 템플릿화 할 수 있는데, @PathVariable 을 사용하면 매칭 되는 부분을 편리하게 조회할 수 있다.

```java
@GetMapping("/mapping/users/{userId}/orders/{orderId}") 
public String mappingPath(@PathVariable String userId, @PathVariable Long orderId) { 
	log.info("mappingPath userId={}, orderId={}", userId, orderId); 				return "ok";
}
```

변수명이 같으면 생략 가능 
@PathVariable("userId") String userId -> @PathVariable userId

## @RequiredArgsConstructor --> 생성자 주입
@RequiredArgsConstructor final 이 붙은 멤버변수만 사용해서 생성자를 자동으로 만들어준다. 

```JAVA
public BasicItemController(ItemRepository itemRepository) 
{
	this.itemRepository = itemRepository; 
} 
```

이렇게 생성자가 딱 1개만 있으면 스프링이 해당 생성자에 @Autowired 로 의존관계를 주입해준다. 따라서 final 키워드를 빼면 안된다!, 그러면 ItemRepository 의존관계 주입이 안된다.

이점: 새로운 필드를 추가할 때 다시 생성자를 만드는 번거로움을 없앨 수 있다.

## @CookieValue
@CookieValue를 이용하면 Cookie 객체를 받을 수 있다.



## @SessionAttribute
 세션 데이터에 접근할 때 사용한다.
	예를 들어서 세션 체크로직이 있어야할 때 세션 을 꺼내서 
	HttpSession session 파라미터로 받아서
	session을 꺼내서 이런식으로 복잡한 과정을 저걸로 통합해준다.
	참고로 세션을 생성하지 않는다 이 어노테이션은

```java
public String homeLoginV3Spring(  
@SessionAttribute(name = SessionConst.LOGIN_MEMBER, required = false) Member loginMember, Model model) {  
//세션에 회원 데이터가 없으면 homeif (loginMember == null) {  
return "home";  
}    
//세션이 유지되면 로그인으로 이동  
model.addAttribute("member", loginMember);  
return "loginHome";  
}
```
## Spring Controller required 속성
 
 필수가 아닌 파라미터인 경우, required 속성 값을 주어 false로 지정해주면 된다.
 required 속성 값을 따로 작성하지 않을 경우, 기본 값은 true로 지정되어 있다.

```java
 @RequestParam(value="query", required=false) String query
```
---
sticker: emoji//1f600
---
[[래퍼 클래스]]

## Optional이란?

T타입 객체의 래퍼클래스

---

## 왜 쓰는가? 
1. null을 직접 다루는 것은 위험하다 (NullpointEx) 발생 방지
2. 간접적으로 null을 다루기 위해서
3. null을 다루게 된다면 null 체크를 한다 --> 이느 if 문 남발 코드 지저분 -> 방지 
4. 다양한 메소드들을 제공 null체크 방지 관련

---
>[!example]
>str 0x100 -> "abc"
optVal 0x200 -> 0x100
>String str="abc"
  Optional<String> optVal = Optional.of(str);

## Optional 객체 생성 하는 법

```java
String str="abc";  
Optional<String> optional1 = Optional.of(str);  
Optional<String> optional2 = Optional.of("abc");  
//Optional<String> optional3=Optional.of(null);  
Optional<String> optional4=Optional.ofNullable(null);  
  
Optional<String> optional5=Optional.empty();
```  
Optional을 초기화 할때는 빈 옵셔널로 초기화 하자
---


```java 
String str1=optional4.orElse("dddd"); //Null일 때는 무엇을 가지고 올래? (파라미터 값)  
System.out.println(str1);  
String str2=optional1.orElseGet(()->new String()); //--Suplier 람다식 사용가능  
System.out.println(str2);

```
//TODO
위의 두개의 명확한 차이 공부하기 결국에는 두개다 NULL일때 어떤식으로 처리할지를 결정하는건데  orElse는 파라미터의 기본값을 넘겨주는거고 orElseGet도 결국 Supplier를 통해 값을 넘겨주는건데 무슨 차이인가?
My think:활용도적인 차이인거 같다.
real response:


---

```java
optional4.ifPresent(System.out::println); //널이 아닐떄만 안에 작업을 수행하도록한다. null이라면 아무일도안한다.
```

---

``` java
int result2=Optional.of("")  
.filter(x->x.length()>0)  
.map(Integer::parseInt)  
.orElse(-1);
```
의문점
왜 빈문자열인데  -1이 출력이  되는거지?
결국 orElse도 null이어야지 -1출력이 된다 했는데.. --> 내부로직 확인
filter 부분에서
return predicate.test(value) ? this : empty(); 여기서 걸리게 된다 그렇게 되면 empty() 함수를 부르게 되고

```java
public static<T> Optional<T> empty() {   
Optional<T> t = (Optional<T>) EMPTY;  
return t;  
}
```
empty함수에서 EMPTY로 가본다 -->

```java
private static final Optional<?> EMPTY = new Optional<>();
```
엥? 빈객체가 생성이 되는데? -->

```java
private Optional() {  
this.value = null;  
}
```
다시 생성자 초기화를 통해 value를 null값으로 세팅한다.  궁금증 해결

이 부분에서 빈 문자열 로 Optional을 생성한다. 그러면 필터에서 걸리게 된다. 
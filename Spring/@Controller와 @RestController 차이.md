# @Controller와 @RestController의 차이
인프런의 스프링 강의에서는 MVC 패턴을 활용해 작성한 어플리케이션 코드의 컨트롤러에 @Contoller 어노테이션을 사용한다. <br>
한편, Spring 을 이용한 Rest API의 예시 코드를 검색해보면 컨트롤러에 @RestContoller를 사용하는 경우가 대부분이다. <br>
이 둘의 차이에 대해 찾아본 결과를 정리한다.

---
## @Controller
전통적인 Spring MVC 컨트롤러인 @Controller는 View를 반환하기 위해 사용합니다. 


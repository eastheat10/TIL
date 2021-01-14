# Spring Form Validation2

  

### validation message 수정

* src/main/resources에 **ValidationMessage.properties** 파일을 만든다

  ```properties
  javax.validation.constraints.Size.message=크기가 {min} 이상 {max} 이하이어야 합니다.
  javax.validation.constraints.Email.message=이메일 주소가 바르지 않습니다.
  javax.validation.constraints.NotEmpty.message=필수 입력항목입니다.
  ```

  

### 개별 항목 에러메세지 등록

* **@NotEmpty** 항목이 여러개 있을 때 서로 다른 에러메세지 출력하기
* Entity 객체 수정

```java
@NotEmpty
@Size(min=6, max=12)
@Min(1)
```

* 위의 코드를 아래 처럼 바꾼다.

```java
@NotEmpty(message="학번을 입력하세요.")
@Size(min=6, max=12, message="6 자리 이상 12 자리 이하")
@Min(value=1, message="양의 정수 입력")
```

* 값이 한개인 어노테이션에서 그 값의 어트리뷰트는 **value**

​    

### 로직 에러 처리하기

* Validation annotation에 의해서 자동으로 검사되기 힘든 에러들을 따로 구현

* bindingResult 객체에 에러메세지 등록하기

  ```java
  bindingResult.rejectValue("passwd2", null, "비밀번호가 일치하지 않습니다.");
  ```

  * **rejectValue** 메소드를 호출하여 에러메세지 등록
    * 첫째 파라미터: 에러가 발생한 속성의 이름
    * 셋째 파라미터: 에러메세지

  ```html
  <form:errors path:"passwd2" class="error" />
  ```

  * 위 태그에 에러메세지 출력

  * UserService.java

  ```java
  public boolean hasErrors(UserRegistration userRegistration, BindingResult bindingResult){
    if (bindingResult.hasErrors())
              return true;
          if (userRegistration.getPasswd1().equals(userRegistration.getPasswd2()) == false) {
              bindingResult.rejectValue("passwd2", null, "비밀번호가 일치하지 않습니다.");
              return true;
          }
          User user = userRepository.findByUserid(userRegistration.getUserid());
          if (user != null) {
              bindingResult.rejectValue("userid", null, "사용자 아이디가 중복됩니다.");
              return true;
          }
          return false;
  
  }
  ```

  * 위의 코드를 *Controller* 에서 호출하여 사용함

    ```java
    if(userService.hasErrors(userRegistration, bindingResult)){
      ...
    }
    ```

    
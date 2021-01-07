# Spring Form Validation

[source](https://github.com/eastheat10/spring/tree/master/form4)



### 입력 Form Submit 과정

* **spring form validation** 기능이 구현되었을 때 입력폼의 **submit**과정은 다음과 같다.

  1. 사용자가 웹브라우저에서 입력 폼에 데이터를 입력하고 **submit**한다.

  ```html
  <form method="post"> 
    <input type="text" name="studentNo" /> 
    <input type="text" name="name" />  		
    <button type="submit">저장</button> 
  </form>
  ```

  2. 서버의 URL이 요청된다 (http request) 

     입력폼에 입력 된 데이터도 이 요청에 같이 담겨 전송된다(request parameter)

     ```json
     studentNo: "201733027"
     name: 		 "홍길동"
     ```

  3. spring web mvc 엔진이 그 요청을 받아서 요청된 URL과 일치하는 액션메소드를 찾는다.

  ```java
  @RequestMapping(value="studentEdit", method=RequestMethod.POST) 
  public String studnetEdit(Model model, @Valid Student student, BindingResult bindingResult) {
    . . . 
  }
  ```

  4. 위 액션 메소드의 파라미터가 객체이기 떄문에, spring web mvc 엔진이 아래의 일들을 자동으로 처리한다.

     * Student 객체 생성

       ```java
       Student student = new Student()
       ```

     * 생성된 객체에 **request parameter** 데이터를 채운다.

       ``` java
       student.setStudnetNo("201733027");
       student.setName("홍길동")
       ```

     * 생성된 객체를 모델 객체에 등록한다

       ```java
       model.addAttribute("student", student);
       ```

     * Student 객체에 채워진 데이터에 문제가 없는지 검사한다

       데이터를 검사할 규칙이 Student 클래스에 등록되어 있어야 한다.

       ```java
       public class Student { 
         @NotEmpty @Size(min=9, max=12) 
         String studentNo; 
         
         @NotEmpty @Size(min=2) 
         String name; 
       }
       ```

       * **@NotEmpty** 어노테이션은 이 멤버 변수 값이 꼭 있어야한다는 뜻이다.
       * **@Size** 어노테이션은 문자열의 최소&최대길이를 지정한다.
       * Student 객체에 채워진 데이터가 어노테이션 규칙에 맞는지 spring web mvc 엔진이 검사하고 검사 결과가 BindResult 객체에 등록된다.

     * 액션메소드가 호출된다

       ```java
       studentEdit(model, student, bindingResult)
       ```



### Model Class

* **request parameter 데이터**를 채우기 위한 객체

  * 입력폼 submit 과정에서 사용된 Student 클래스처럼 

    request parameter 데이터를 채우기 위한 클래스를 모델 클래스라고 부른다.

    ```java
    @RequestMapping(value="studentEdit", method=RequestMethod.POST)
    public String studentEdit (Model model, Student student){
      ...
    }
    ```

* model 객체에 채워져 **뷰에 전달되기 위한 객체**

  ```java
  @RequestMapping(value="studentEdit", method=RequestMethod.POST)
  public String studnetEdit(Model model){
    Student student = new Student();
    model.addAttribute("student", student);
    return "studentEdit";
  }
  ```

  * 위의 코드에서 student 객체는 model 객체에 채워져서 studentEdit.jsp 뷰에 전달된다.
  * 이런 용도로 사용되는 클래스를 모델 클래스라고 부른다.

  * spring form validation 규칙을 설정하기 위한 어노테이션은 모델 클래스에 추가해야 한다(@NotEmpty, @Size ...)

  

### Entity Class

* Jpa 에서 구현하게되는 @Entity 어노테이션이 붙은 클래스

* 엔터티 객체는 JPA Repository의 조회 결과 데이터를 리턴할 때 사용되는 객체이다.

  ```java
  Student student = studentRepository.findByStudentNo("123123");
  ```

* JPA Repository의 sava 메소드를 호출하여 데이터를 저장할 때에도 엔터티 객체를 사용한다.

  ```java
  studentRepository.save(student);
  ```

  

### 컨트롤러 클래스와 서비스 클래스

* 컨트롤 클래스는 서비스 클래스만 참조하고 서비스 클래스의 메소드를 호출해야한다.

* 테이블에 대해 직접 CRUD작업을 할 경우 DAO클래스를 구현하고 DAO클래스 내부에서 mapper 또는 Repository를 사용해야 한다.

* 클래스 객체를 검사, 계산 또는 변경 등의 작업은 Service클래스에서 구현해야 한다.

  

  **컨트롤러 클래스** -> **서비스 클래스** -> **DAO 클래스**



### pom.xml

```xml
<dependency> 
  <groupId>org.springframework.boot</groupId> 
  <artifactId>spring-boot-starter-validation</artifactId> 
</dependency>
```

* Java11은 spring validation 라이브러리가 기본적으로 포함되지 않기 때문에 위의 maven dependency가 필요하다.




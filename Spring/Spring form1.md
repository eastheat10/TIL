# Spring form1

[source](https://github.com/eastheat10/spring/tree/master/form1)



## select 태그 구현

1. 모델 객체

   * Spring mvc: Model, View, Controller

   * Model은 컨트롤러와 뷰 사이에 전달되는 데이터 객체

     * 컨트롤러 액션메소드에서 뷰에게 전달하는 데이터
     * 뷰의 실행 결과 출력이 웹 브라우저 창에 전달되어 그려질 떄 입력폼에 채워져 있는 데이터
     * 웹브라우저 입력폼 내부의 submit 버튼이 클릭되어 입력폼에 입력된 데이터가 서버의 액션 메소드의 파라미터로 전달되는 데이터

     

2. 데이터 객체
   * Entity
   * DTO, VO
     * mybatis에서 사용
     * DB에서 조회한 데이터 전달하기위해 구현
     * 컨트롤러와 뷰 사이에 데이터를 전달하는 용도로 사용하기도 한다.(model이 더 적절한 용어)



---



```java
@ModelAttribute
public void addSomething(Model model){
	model.addAttribute("departments", departmentRepository.findAll());
}
```

* `@ModelAttribute` 어노테이션이 붙은 메소드의 본문은 이 컨트롤러의 모든 액션메소드에 적용



```html
<form:select path="departmentId1" itemValue="id" itemLabel="name" items="${ departments }" />
```

만약 모델객체의 departmentId 속성값이 4 라면 출력결과는 밑의 코드와 같다.

```html
<select id="departmentId1" name="departmentId1">
  <option value="1">소프트웨어공학과</option>
  <option value="2">컴퓨터공학과</option>
  <option value="3">정보통신공학과</option>
  <option value="4" selected>글로컬IT공학과</option>
</select>

```



### Radio Button

```html
<form:radiobuttons path="departmentId1" itemValue="id" itemLabel="shortName" items="${ departments }" />
```

모델 객체의 departmentId 속성 값이 4라면 위 확장태그의 실행결과 출력은 다음과 같다

```html
<span>
  <input id="departmentId11" name="departmentId1" type="radio" value="1"/>
  <label for="departmentId11">소프</label>
</span>
<span>
  <input id="departmentId12" name="departmentId1" type="radio" value="2"/>
  <label for="departmentId12">컴공</label>
</span>
<span>
  <input id="departmentId13" name="departmentId1" type="radio" value="3"/>
  <label for="departmentId13">정통</label>
</span>
<span>
  <input id="departmentId14" name="departmentId1" type="radio" value="4" checked />
  <label for="departmentId14">글티</label>
</span>
```

* name 값이 같은 radio들은 하나의 그룹이 된다. 그 그룹에서 하나만 선택 가능하다.
* radio의 value 값이 request parameter로 전달된다.





### Check Box

```html
<form:checkboxes path="departmentId4" itemValue="id" itemLabel="shortName" items="${ departments }" />
```

모델 객체의 departmentId4 속성 값이 4 이면 출력은 다음과 같다

```html
<span>
  <input id="departmentId41" name="departmentId4" type="checkbox" value="1"/>
  <label for="departmentId41">소프</label>
</span>
<span>
  <input id="departmentId42" name="departmentId4" type="checkbox" value="2"/>
  <label for="departmentId42">컴공</label>
</span>
<span>
  <input id="departmentId43" name="departmentId4" type="checkbox" value="3"/>
  <label for="departmentId43">정통</label>
</span>
<span>
  <input id="departmentId44" name="departmentId4" type="checkbox" value="4" checked />
  <label for="departmentId44">글티</label>
</span>

```

* name이 같은 check box는 여래 개 체크할 수 있다. 체크된 check box 값이 request parameter가 되어 전달된다.

* 그래서 이 값을 전달 받는 변수는 **배열**이어야 한다.

  ```java
  @Data
  public class Form1 {
      int departmentId1;
      int departmentId2;
      int departmentId3;
      int[] departmentId4;
      int[] departmentId5;
      boolean enabled;
  }
  ```



``` html
<form:checkbox path="departmentId5" value="1" label="소프" />
<form:checkbox path="departmentId5" value="2" label="컴공" />
<form:checkbox path="departmentId5" value="3" label="정통" />
<form:checkbox path="departmentId5" value="4" label="글티" />
```

departmentId5 속성의 값이 {1, 3}  **배열**이면 출력은 다음과 같다

```html
<input id="departmentId51" name="departmentId5" type="checkbox" value="1" checked />
<label for="departmentId51">소프</label>

<input id="departmentId52" name="departmentId5" type="checkbox" value="2"/>
<label for="departmentId52">컴공</label>

<input id="departmentId53" name="departmentId5" type="checkbox" value="3" checked />
<label for="departmentId53">정통</label>

<input id="departmentId54" name="departmentId5" type="checkbox" value="4"/>
<label for="departmentId54">글티</label>
```

이 체크박스가 체크되면, 이름=enable, 값="true" request parameter가 서버에 전달된다.

boolean 타입 변수에 대입된다.
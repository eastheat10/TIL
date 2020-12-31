# Spring form2

[source](https://github.com/eastheat10/spring/tree/master/form2)



### countBy

---

```java
int countByDepartmentId(int departmentId);
```

```sql
SELECT COUNT(*) FROM student WHERE departmentId = #{departmentId}
```

* query creation 기능에 의해서 자동 구현

* 자바 메소드의 리턴값은 SQL 명령과 같음



### INSERT 구현

---

``` java
@RequestMapping("list")
public String list(Model model) {
  model.addAttribute("departments", departmentRepository.findAll());
  return "form2/list";
}

@PostMapping("insert")
public String insert(Model model, Department department) {
  departmentRepository.save(department);
  return "redirect:list";
}
```

* list 액션 메소드
  *  학과 목록 화면 출력
* insert 액션 메소드
  * 학과를 DB에 등록(insert)하고 학과 목록 화면으로 넘어간다



```html
<form method="post" action="insert">
    <table class="list condensed">
     •••
        <tr>
           <td></td>
           <td><input type="text" name="name" style="width: 250px;" /></td>
           <td><input type="text" name="shortName" style="width: 80px;" /></td>
           <td><input type="text" name="phone" style="width: 120px;" /></td>
           <td><button type="submit" class="btn">저장</button></td>
        </tr>
      </tbody>
    </table>
  </form>
```

* 오직 ```<td>``` 태그 내부에만 다른 태그가 들어갈 수 있다.
  *  ```<table>```  , ```<tr>``` 태그 사이에 다른 태그가 들어가면 안됨.
  * ```<form>``` 태그는 ```<table>```  태그 바깥에서 ```<input>``` 태그 들을 감싸야 한다.



```html
<form method="post" action="insert">
```

* **submit**  버튼이 클릭되면 **insert** 메소드가 호출된다.
* **insert** 액션메소드는 학과를 DB에 등록하고 학과목록화면으로 넘어감



### DELETE 구현

---

```java
@RequestMapping("delete")
public String delete(Model model, int id) {
  departmentRepository.deleteById(id);
  return "redirect:list";
}
```

* id 값으로 레코드를 찾아서 삭제하고, 목록화면으로 넘어간다.

  * **Referential Integrity Constraint Violation 에러**

    * <u>foreign key</u>로 참조되고 있는 레코드를 삭제할 수 없기 때문에 에러가 발생

      > 소속학생이 있는 학과는 삭제할 수 없다.

    * 참조무결성제약 = 외래키 제약

    

```java
@RequestMapping(value="process", method=RequestMethod.POST, params="cmd=insert")
public String insert(Model model, Department department) {
```

* **POST** 방식으로, process URL이 요청되었고, request parameter에 <u>cmd=insert</u> 항목이 있을 때 이 액션메소드가 호출된다.

```java
@RequestMapping(value="process", method=RequestMethod.POST, params="cmd=delete")
public String delete(Model model, int[] idChecked) {
```

*  **POST**  방식으로, process URL이 요청되었고, request parameter에 <u>cmd=delete</u> 항목이 있을 때 이 액션 메소드가 호출된다.
   	* 위 두 액션 메소드를 호출하는 수단은 url, request method 둘 다 같고, request parameter 값만 다르다

```html
  <button type="submit" class="btn-mini" name="cmd" value="delete" data-confirm-delete>삭제</button>
```

* 이 submit 버튼을 클릭하면, request parameter에 cmd=delete 항목이 추가된다.

```html
<button type="submit" class="btn" name="cmd" value="insert">저장</button>
```

* 이 submit 버튼을 클릭하면, request parameter에 cmd=insert 항목이 추가된다.



### UPDATE 구현

___

```java
@RequestMapping(value="process", method=RequestMethod.POST, params="cmd=save")
public String save(Model model, Form5 form5) {
  for (int i = 0; i < form5.getId().length; ++i) {
    Department department = new Department();
    department.setId(form5.getId()[i]);
    department.setName(form5.getName()[i]);
    department.setShortName(form5.getShortName()[i]);
    department.setPhone(form5.getPhone()[i]);
    if (department.getName().trim().length() > 0)
    	departmentRepository.save(department);
  }
	return "redirect:list";
}

```

* 입력 폼에 입력된 값이 form5객체에 채워져 전달된다.
* JPA save 메소드의 파라미터는 entity 객체이어야 한다.
  * Department 엔터티 객체를 생성하고 모델 객체의 속성 값을 엔터티 객체에 채운다.
* department.id 값과 일치하는 레코드가 DB에 있으면 그 레코드가 **update** 된다.
* 일치하는 레코드가 없으면 새 레코드가 **insert** 된다.



##### JPA repository의 save 메소드

1. 저장할 엔터티 객체의 기본키(id) 속성값이 null 이거나 0 이면

   1. 그 엔터티를 DB에 **INSERT** 한다.
   2. **return** 한다.

2. 저장할 엔터티 객체의 기본키(id) 속성값으로 DB에서 조회(SELECT) 한다.

3. 조회된 레코드가 있다면

   1. 방금 조회한 레코드 값과, 저장할 엔터티 객체의 속성값을 비교한다.

   2. 비교 결과 값이 다른 부분이 있으면 **UPDATE** 명령을 실행한다

      > 모든 속성 값이 동일하면 **UPDATE** 하지 않는다.

   3. **return** 한다.

4. 조회된 레코드가 없다면
   1. 그 엔터티를 DB에 **INSERT** 한다.
   2. **return** 한다


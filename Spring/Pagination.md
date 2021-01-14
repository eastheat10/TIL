# Pagination

 [Source code](https://github.com/eastheat10/spring/tree/master/page1) 

  

* 액션 메소드의 파라미터가 객체일 경우

  1. 그 객체에 request parameter 데이터가 자동으로 채워진다.

  2. 그 파라미터 객체가 모델에 자동으로 채워진다

     * 모델 데이터의 이름은 객체의 이름이다.(첫 글자만 소문자로)

     

### Pagination.java

```java
import lombok.Data;

@Data
public class Pagination {
    int pg = 1;        // 현재 페이지. 디폴트 값은 1
    int sz = 15;       // 페이지 당 레코드 수. 디폴트 값은 15
    int recordCount;   // 전체 레코드 수

    public String getQueryString() {
        return String.format("pg=%d&sz=%d", pg, sz);
    }
}
```

* 페이지 단위 조회 기능 구현을 위해 만든 모델 객체
* http://localhost:8088/student/list?pg=3&sz=15
  * 위의 URL을 서버에 요청했을 때 query string 값 들은 request parameter가 되어 서버에 전송된다.
  * 이 request parameter 값은 액션 메소드의 파라미터 Pagination 객체에 채워진다.



### StudentRepository.java

```java
import java.util.List;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Sort;
import org.springframework.data.jpa.repository.JpaRepository;

import net.skhu.entity.Student;
import net.skhu.model.Pagination;

public interface StudentRepository  extends JpaRepository<Student, Integer> {

    public default List<Student> findAll(Pagination pagination) {
        Page<Student> page = this.findAll(PageRequest.of(pagination.getPg() - 1, 																		pagination.getSz(), Sort.Direction.ASC, "id"));
        pagination.setRecordCount((int)page.getTotalElements());
        return page.getContent();
    }
}
```

* ```java
  public default List<Student> findAll(Pagination pagination)
  ```

  * 페이지 단위 조회 기능을 구현한 메소드
  * 자동으로 구현된 ```Page<Student> findAll(Pageable pageable);``` 메소드를 사용하여 구현.
  * Pagination 모델 객체에 담겨있는 페이지 단위 조회 조건에 따라 레코드를 조회하여 리턴.
  * ```pagination.getPg();```으로 조회할 페이지 번호 가져온다. 1부터 시작.

  

###  Controller

* pagination을 구현한 모든 페이지의 URL의 query string에 현재 페이지 번호가 포함되어야 한다.

* 페이지 크기를 쉽게 바꿀 수 있도록 페이지 크기도 URL의 query string에 포함되는 것이 좋다.

  > http://localhost:8088/student/list?**pg=3&sz=15**
  >
  > > 현재 페이지가 3, 페이지 크기가 15

* 위의 URL들을 서버에 요청하면 pagination 정보가 request parameter로 전달된다.

* pagination 정보 request parameter를 전달받기 위한 Pagination 모델 객체가 필요하다.

* 목록으로 돌아가기 위한 redirect URL은 ```return "redirect:list?" + pagination.getQueryString();```

* 마지막 페이지(페이지 크기) 구하는 방법

  ```java
  int lastPage = (int)Math.ceil((double)studentRepository.count() / pagination.getSz());
  ```

  

### View

* 등록버튼

  ```html
  <a href="create?${pagination.quertString}" class="btn">등록</a>
  ```

* 수정버튼

  ```html
  <tr data-url="edit?id=${student.id}&${pagination.queryString}" ></tr>
  ```

* 삭제버튼

  ```html
  <button type="submit" class="btn" name="cmd" value="delete">
    삭제
  </button>
  ```

* 목록으로

  ```html
  <a href="list?${pagination.queryString}" class="btn">목록으로</a>
  ```

* pagination 버튼

  ```html
  <my:pagination pagiSize="${ pagination.sz }" recordCount="${ pagination.recordCount }" queryStringName="pg" />
  ```

  * 페이지 번호 목록 출력

  * **${ pagination.sz }** : pagination 모델 객체의 sz 속성은 페이지 크기이다.

  * **${ pagination.recordCount } **: pagination 모델 객체의 recordCount 속성은 학생 레코드 수이다.

    페이지 번호 목록을 출력하려면 레코드 수가 필요하다. (페이지 수 = 레코드 수 / 한 페이지 당 크기)


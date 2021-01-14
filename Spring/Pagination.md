# Pagination

  

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

  페이지 단위 조회 기능을 구현한 메소드
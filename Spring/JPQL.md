# JPA query language

[source](https://github.com/eastheat10/spring/tree/master/jpa5)



###  @JsonIgnore

* rest controller 액션메소드가 리턴하는 데이터는 JSON형식의 문자열로 변환되어 웹브라우저에 전송된다.

* 이 어노테이션을 붙이면 그 속성은 변환되지 않음

  > Student를 json으로 변환할 때 department는 포함되어야 하지만 Department를 변환할 때에는 Student를 포함하면 안된다. (순환참조)



### JPQL (jpa query language)



**JPQL**

* JPQL 문법은 엔터티 객체들을 조회한다

  ```sql
  SELECT d
  FROM Department d -- List<Department> 객체를 리턴한다(전체 Department 객체)
  ```

  ```sql
  SELECT s
  FROM Student s
  WHERE s.department.id = 1
  -- Student 엔터티 클래스에는 departmentId 속성이 없고 department 속성이 있다
  ```

  ```sql
  SELECT s FROM Student s 
  WHERE s.department.id = 1 
  OR s.department.id = 2 -- WHERE s.department.id IN(1, 2) 
  ORDER BY name
  ```

  

* 조인 방법 (single valued association)

  * #### **SQL**

    * 학생목록을 조회할 때 조회 결과에 학과명을 포함하려면 Stdudent, Department 조인해야 함

      ``` sql
      SELECT s.studentNo, s.name, d.name
      From Student s LEFT JOIN DEPARTMENT d ON s.departmentId = d.id
      ```

      

  * JPQL

    * Student Entity 클래스에는 department 멤버변수가 포함되어 있다.

      `Department department` 

    * 그래서 JPQL로 구현 시 Department와 조인할 필요가 없다

      ```sql
      SELECT s.studentNo, s.name, s.department.name
      FROM Student s
      ```

    * Student 엔터티 클래스에는 sugangs속성이 포함되어 있다.

      `List<Sugang> sugangs;`

    * Sugang 엔터티 클래스에는 lecture 속성이 포함되어 있다.

      `Lecture lecture`

    * '201532006'학번 학생이 수강(sugang)하고 있는 강좌(lecture) 이름을 조회하는 JPQL 명령은 다음과 같다

      ```sql
      SELECT g.lecture.title
      FROM Student s JOIN s.sugangs g
      WHERE g.student.studnetNo = '201532006'
      ---
      SELECT g.lecture.title
      FROM sugang g
      WHERE g.student.studentNo = '201532006'
      ```

       

* Group By

  * 학과별 학생수를 조회한다

    ```sql
    SELECT s.department, COUNT(s)
    FROM Student s
    GROUP BY s.department
    ```

  * 강좌별 학생 수를 조회한다

    ```sql
    SELECT g.lecture.title\, COUNT(g)
    FROM Sugang g
    GROUP BY g.lecture
    ```

    

* JPQL 함수

  | 함수                                | 내용                                                         |
  | ----------------------------------- | ------------------------------------------------------------ |
  | ABS(number)                         | 절대값을 리턴한다                                            |
  | CONCAT(String1, String2)            | 두 문자열을 연결하여 리턴한다                                |
  | CURRENT_DATE()                      | 현재 날짜를 리턴                                             |
  | CURRENT_TIME()                      | 현재 시각을 리턴                                             |
  | INDEX(identification_variable)      | identification_variable이 참조하는 엔터티의 컬랙션에서 인덱스를 리턴 |
  | LENGTH(string)                      | 문자열 길이를 리턴                                           |
  | LOCATE(String1, String2, [, start]) | string2 문자열에서 string1 문자열의 위치를 리턴                 start 값이 주어지면 start 위치부터 찾기 시작한다.           start1 문자열 start2 문자열의 사작부분과 일치하면1 리턴 or 0 리턴 |
  | LOWER(String)                       | 문자열을 소문자로 변환하여 리턴한다                          |
  | MOD(number1, number2)               | number1을 number2로 나눈 나머지를 리턴                       |
  | SIZE(collection)                    | 컬렉션의 크기를 리턴. 컬렉션에 포함된 객체의 수 리턴         |
  | SQRT(number)                        | 제곱근 리턴                                                  |
  | SUBSTRING(String, start, end)       | 부분 문자열 리턴                                             |
  | UPPER(String)                       | 문자열을 대문자로 변환하여 리턴한다                          |
  | TRIM(String)                        | 문자열 앞 뒤의 공백을 제거하고 리턴한다                      |



* UPDATE / DELETE

  * JPA Repository 기본 메소드 save로 INSERT 대체 가능

  * query create 기능이 충분하기 때문에 DELETE 사용할 일 거의 없음

  * 대부분 SELECT, UPDATE 명령만 구현하면 됨

    

  * query creation 메소드의 예

    >void delete(int id)
    >
    >void deleteByName(String name)
    >
    >void deleteByStudentNo(String studentNo)
    >
    >void deleteByNameOrStudentNo(String name, String studentNo)
    >
    >void deleteByDepartmentName(String departmentName)
    >
    >void deleteByDepartmentId(int departmentId)
    >
    >> 위 메소드들의 이름이 JPA query creation 규칙에 맞아서 메소드 자동구현

  
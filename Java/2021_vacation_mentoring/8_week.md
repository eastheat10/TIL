# SM 8주차

​		

### 목표

---

* 자바의 상속



### 목차

---

1. 자바 상속의 특징
2. super 키워드
3. 메소드 오버라이딩
4. 추상 클래스
5. final 키워드
6. Object 클래스





#### 1. 상속

---

* 자식 클래스가 부모 클래스를 선택하여, 그 부모의 멤버(필드, 메서드)를 상속받아 그대로 쓸 수 있다.
* 이미 마련되어 있는 클래스를 재사용하여 만들기 때문에 효율성과 시간개선에 효과적이다.
* **private**키워드가 있는 필드와 메서드는 상속받을 수 없다.
* 부모클래스와 자식클래스의<u> 패키지가 다르다면</u> **default** 멤버는 상속받지 못한다.
* **extends** 키워드를 사용해서 상위 클래스를 상속받는다.



```java
public class Parent{
  private String first_name;
  public String last_name;
  
}

public class Child extends Parent {
	private first_name;
  // last_name 은 상속받음
}
```





#### 2. super 키워드

---

* 자바에서 자식객체를 생성하면 부모객체 생성 후 자식 객체가 그 다음에 생성된다.
* 객체 생성 시 명시적 생성자가 없으면 기본 생성자를 생성하여 호출한다.
* 부모 클래스에 명시적 생성자가 없으면 자동으로 **super();**를 호출하여 실행한다.
* 반드시 자식 생성자 첫 줄에 super( ... );를 작성해야지만 에러가 발생하지 않는다.

```java
public class Parent{
  public String last_name;
  
  public Parent(String last_name){
    this.last_name = last_name;
  }
}

public class Child extends Parent {
	private first_name;
  // last_name 은 상속받음
  
  public Parent(String first_name, String last_name){
    super(last_name);
    this.first_name = first_name;
  }
}
```





#### 3. 메소드 오버라이딩

---

* 자바의 다형성을 지원하는 방법으로 메소드 오버로딩(Overloading)과 오버라이딩(Overriding)이 있다.

1. 오버로딩

   * 같은 이름의 함수를 여러 개 정의하고 매개변수의 유형과 개수를 다르게하여 다양한 유형의 호출에 응답한다.

   ```java
   int sum(int a, int b){
     return a + b;
   }
   
   int sum(double x, double y, double z){
     return x + y + z;
   }
   
   public static void main(String[] args){
     sum(10, 20);
     sum(5.5, 10.5, 15.5);
   }
   ```

   * 위와 같이 **sum** 이라는 메서드를 여러 개 정의하고 매개변수만 변경하여 선언했을 때, 호출되는 매개변수에 따라 함수가 실행된다.

     

2. **오버라이딩**

   * 부모 클래스로부터 상속받은 메서드를 다르게 정의해야할 때 사용한다. (메서드 재정의)
   * 자식클래스에서 메서드를 재정의 하기 위해서는 메서드 이름, 리턴 타입, 매개변수 개수, 매개변수의 자료형과 순서를 동일하게 지켜야한다.
   * 오버라이딩하는 메서드의 접근제어자는 부모클래스보다 좁게 설정할 수 없다

   * 주로 **@Override** 라는 어노테이션을 사용한다. 사용하지 않아도 되지만, 어노테이션을 통해 오류를 줄일 수 있다.

   ```java
   public class Army{
     
     String rank;
     
     public void work(String rank){
       System.out.println(rank);
     }
   }
   
   public class 일병 extends Army{
     
     @Override
     public void work(String rank){
       System.out.println(rank + "work");
     }
     
   }
   
   public class 병장 extends Army{
     
     @Override
     public void work(String rank){
       System.out.println(rank + "run!");
     }
     
   }
   ```



#### 4. 추상 클래스

---

* 추상클래스는 여러개의 클래스에서 공통점(필드, 메서드)을 추출하여 만든 클래스이다.
* 다수의 개발자가 동일한 목적으로 클래스를 설계해도 같은 모양의 클래스가 나올 수 없다. 그래서 추상클래스를 상속하여 통일성을 높여서 유지보수를 쉽게 할 수 있다.
* 추상클래스는 반드시 abstract 키워드를 사용해야 한다.

```java
public abstract class Hello{ ... }
```

* 추상 메서드가 필요하나 없어도 된다.

  * 추상메서드는 코드가 구현되지않고 형식만 있는 메서드다
  * 메서드의 반환타입, 이름, 매개변수가 필요하다.

  ```java
  public abstract void hello(String name, String message);
  ```

* 추상클래스를 상속받은 실체클래스들은 반드시 추상메서드를 오버라이딩해야한다.

  ​	

#### 5. final 키워드

---

* final 키워드는 주로 상수라는 뜻으로 사용된다.
* 클래스에 사용되면 상속될 수 없는 클래스이다.
* 메서드에 사용되면 오버라이딩할 수 없는 메서드이다.
* 변수에 사용되면 상수가 되어 변경이 불가능한 변수가 된다.
  * 변수에 선언이 되면 상수가 되므로 반드시 생성 시 초기값을 지정해야 된다.
  * 변수 선언 시 초기화 하거나 생성자를 통해 초기화 해야한다.
  * 외부 클래스에서 사용 시 클래스 이름을 명시해주어야 한다.   ``` 클래스명.final변수명 ```



#### 6. Object 클래스

---

* Object 클래스는 모든 클래스가 상속받는 최상위 클래스이다.

* 모든 클래스는 Object클래스의 메서드를 사용할 수 있고 일부 메서드는 오버라이딩해서 사용가능하다.

* Object클래스가 들어있는 **java.lang** 패키지는 자동으로 import된다.

* 주로 toString(), equals() 등을 오버라이딩 한다.

* **toString()**

  * ```getClass().getName() + '@' + Integer.toHexString(hashCode())```
  * 어떠한 정보를 문자열형태로 출력할 때 사용한다.

  ```java
  public class Book{
  	String publisher;
    String title;
    String author;
    
    @Override
    public String tostring(){
      return "출판사: " + publisher + " 제목: " + title + " 작가: " + author;
    }
  }
  ```

* **equals**

  * **==** 연산 결과 반환
  * 객체끼리 연산하면 주소값을 비교하기 때문에 객체 내부의 값을 비교하기위해 오버라이딩한다.

  ```java
  public class Member{
    int id;
    String name;
    
    public Member(int id, String name){
      this.id = id;
      this.name = name;
    }
    
    public int getId(){
      return id;
    }
    
    @Override
    public boolean equals(Member member){
      return this.id == member.id
    }
    
    public static void main(String[] args){
      Member mem1 = new Member(1, "mem1");
      Member mem2 = new Member(1, "mem1");
      
      System.out.println(mem1.equals(mem2));
      // true 출력
    }
  }
  ```

  
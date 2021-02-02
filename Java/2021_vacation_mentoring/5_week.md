# SM 5주차

​		

### 목표

---

* 자바가 제공하는 제어문 학습 (control statement)



### 목차

---

1. 조건문
2. 선택문
3. 반복문



#### 제어문

---

* 조건문: if문

* 선택문: switch문

* 반복문: for문, while문

* 분기문: break, continue

  ​	



#### 조건문 (if문)

---

1. if문

   ```java
   if(조건식){
     // 조건식이 참(true)일 경우 실행
   }
   ```

   * 조건식에는 **boolean** 타입의 변수 또는 true/false 값을 리턴하는 연산식이 와야한다.

   

2. if-else문

   ```java
   if(조건식){
     // 조건식이 참(true)일 경우 실행
   } else{
     // 조건식이 거짓(false)일 경우 실행
   }
   ```

   * 조건식이 **true** 일 경우 if문이 실행되고 **false** 이면 else문이 실행된다.
   * 조건식의 결과에 따라 두 블록 중 하나의 블록만 실행되고 if문을 벗어난다.



3. if - else if - else 문

   ```java
   if(조건식1){
     // 조건식1이 참(true)일 경우 실행
   } else if(조건식2){
     // 조건식1이 거짓(false)이고 조건식2가 참(true)일 경우 실행
   } else if(조건식3){
     // 조건식1, 조건식2가 거짓(false)이고 조건식3이 참(true)일 경우 실행
   } else{
     // 모든 조건식이 거짓(false)일 경우 실행
   }
   ```

   * 조건문이 여러개인 다중 if문
   * **else if** 문의 수는 제한이 없다.



#### 선택문 (switch문)

---

* if문과 달리 변수가 어떤 값을 갖는냐에 따라 실행문이 선택.

* if문의 경우의 수가 많아질 수록 switch문을 사용하는 것이 효과적이다.

* 변수에 해당하는 값이 없으면 default문 실행 (default문은 생략가능).

* 변수의 값에 따라 case문이 실행된 후 제어문을 빠져나오기 위해 break 사용.

  ```java
  switch(조건){
    case 1:
      ///	조건이 1과 같으면 실행
      break;
    case 2:	
      ///	조건이 2와 같으면 실행
      break;
    default:
      // 해당하는 조건이 없으면 실행
      // default문 실행 후 switch문 탈출
  }
  ```

  ​	

#### 반복문

---

##### 1. while문

```java
while(조건식){	// 조건식이 true일 경우 실행
  // 반복 될 문장
}
```

​		

##### 2. do-while문

```java
do{
  // 반복 될 문장
} while(조건식);
```

* while 문과 다르게 반복 될 문장을 무조건 한 번 실행 시키고 조건식을 검사한다.
* 반복문 끝에 ;(세미콜론)을 붙여야 한다.



##### 3.for문

```java
for(초기값; 조건식; 증감값){
  // 코드
}
```

* 초기값을 선언하여 조건식이 true일 동안 루프를 반복한다.

* 실행순서

  1. 초기값 초기화

  2. 조건식 검사

  3. 코드 수행

  4. 증감값 수행

  5. 조건식 검사

  6. ....

     ​	

##### 향상된 for문 (for-each문)

```java
public class ForEach{
  public static void main(String[] args){
    int[] array = new int{1, 2, 3, 4, 5};
    for(int i : array){
      System.out.print(i + " ");
    }
  }
}
// 출력결과: 1 2 3 4 5
```

* 주로 배열, List등에 사용된다
* array 배열의 각 요소를 순차적으로 int형 변수 i에 전달한다.

* 반복횟두를 제어할 수 없다.

  ​	



#### 분기문

---

##### 1. break

* 제어문을 탈출한다.

  ```java
  int l = 1;
  int tmp = 1;
  
  while(true) {
    if(tmp >= n)
      break;
    tmp += (6 * l);
    l++;
  }
  ```

  

##### 2. continue

* continue키워드 밑의 코드를 수행하지 않고 다음 루프로 넘어간다.

  ```java
  int sum = 0;
  for(int i = 0; i < 5; i++){
    if(i == 3)		// i == 3이면 sum += i; 를 실행하지 않고 i = 4일 때로 넘어간다.
      continue;
    sum += i;
  }
  ```

  
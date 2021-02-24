# 정렬

[참고한 글](https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html)

[[단어 정렬(백준1181번)]](https://www.acmicpc.net/problem/1181)

주로 배열을 정렬할 때 `Arrays.sort(array);`를 사용한다.

하지만 위의 코드는 문자열일 때에는 사전적 오름차순 , 숫자일 때에는 오름차순 정렬만 가능하다.

사용자가 정렬 기준을 만들어서 정렬할 수 있는 방법은 두가지이다.

## 1. Comparable interface

- java.lang.Comparable

  - Java에서 제공되는 정렬이 가능한 모든 class들은 모두 Comparable 인터페이스를 구현할 수 있다.

- 구현방법

  - 정렬할 객체에 Comparable 인터페이스를 implements 후 compareTo() 메서드를 오버라이딩 하여 구현.
  - compareTo() 메서드 사용방법
    - 현재 객체 < 파라미터 객체: 음수 리턴
    - 현재 객체 == 파라미터 객체 : 0 리턴
    - 현재 객체 > 파라미터 객체 : 양수 리턴
      - 음수 또는 0 이면 객체의 자리가 유지되고, 양수인 경우 두 객체의 자리가 바뀐다.

- 사용방법

  - `Arrays.sort(array);`

  - `Collections.sort(list);`

  - Arrays.sort() 와 Collections.sort()의 차이

    - Arrays.sort()는 내부적으로 3개의 sorting을 사용한다

      1. Insertion Sort
      2. Merge Sort
      3. Quick Sort

      - Insertion + Merge = Tim Sort
      - Insertion + Quick = Dual-Pivot Sort

    - Arrays.sort() 는 Dual Pivot Sort를 사용한다

    - Collections.sort()는 Tim Sort를 사용한다.

    - Quick Sort는 성능은 좋지만 안정적이지 않아서(어떠한 상황에서 성능이 매우 안좋다) 안정적인 상황이 필요한 Object는 Collections.sort()를 사용한다.

    - 찾아본 글

      - [스택오버플로우](https://stackoverflow.com/questions/32334319/why-does-collections-sort-use-mergesort-but-arrays-sort-does-not)
      - [블로그](https://velog.io/@sparkbosing/Java-Arrays.sort와-Collections.sort의-차이)
      - [블로그](https://defacto-standard.tistory.com/38)

    - [내 코드](https://github.com/eastheat10/algorithm/blob/master/src/algo/stage12/A11650_object.java)

      

## 2. Comparator interface

- 이번 문제에서 사용한 방법

- 정렬기준을 바꿔서 정렬할 때 사용하는 인터페이스

- java.util.Comparator

  - 주로 익명클래스로 사용된다.
  - 기본 정렬인 오름차순을 내림차순으로 정렬할 때 사용된다.

- 사용방법

  - Comparator 객체 생성 후 compare()메서드 오버라이딩 해서 사용한다.

  - compare()메서드 사용방법

    - compare(<T> o1, <T> o2){}
    - o1 < o2 : 음수리턴
    - o1 == o2 : 0 리턴
    - o1 > o2 : 양수 리턴
    - 음수 또는 0이면 자리가 유지되고 양수이면 자리 변경
      - `return (x < y) ? -1 : ((x == y) ? 0 : 1);`

    ```jsx
    Arrays.sort(words, new Comparator<String>() {
    			@Override
    			public int compare(String o1, String o2) {
    				if (o1.length() != o2.length()) {
    					return o1.length() - o2.length();	// 글자수 오름차순
    				} else {
    					return o1.compareTo(o2);	// 사전적 오름차순
    				}
    			}
    		});
    /*
     * 글자수가 증가하는 순으로 정렬
     * 글자수가 같으면 사전순으로 정렬
     */
    ```
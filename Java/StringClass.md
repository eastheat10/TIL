# String Class
- - - -
* CharSequence _Interface_
	* 문자열 클래스들의 동통부모 클래스
	1. String 클래스
		* 대표적인 자바 문자열 클래스.
		* String 객체가 생성되면 문자열이 수정될 수 없다
		* 수정하면 새로운 객체를 리턴
	2. StringBuilder 클래스
		* 수정가능한 문자열 객체
		* 클래스 내부의 문자열 수정 메소드는 내부 문자열을 직접 수정
		* thread safe하지 않음
	3. StringBuffer 클래스
		* StringBuilder와 기능이 같고 메소드도 같음
		* thread safe하기 때문에 StringBuilder보다 느리다> thread safe: 멀티스레드로 실행되어도 안전하다.


* 공통 부모 인터페이스의 목적
	* 공통 부모 인터페이스를 구현(implements)한 클래스들은, 인터페이스에 선언 된 메소드를 전부 구현해야 하고 사용법도 같아야 한다.
	* 사용법이 같아서 호환되는 클래스들을 만들기위해 인터페이스 사용


* equals 메소드> Object class에 정의 되어 있음
	* String 클래스는 equals 메소드를 재정의(override)하였다
	* StringBuffer 클래스는 재정의 하지 않고 object 클래스의 equal 메소드 상속
		* equals 메소드르 재정의 하지 않느 클래스는 값을 비교하기 어렵다
		* StringBuilder 클래스는 문자열 보관, 비교, 조회용으로 사용하기 어려움
		* 문자열 조립, 생성시에만 사용하고 조립이 완성되면 toString메소드로 문자열을 String 클래스로 변환하여 사용.
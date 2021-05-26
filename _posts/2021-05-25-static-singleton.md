---
title: "static과 싱글톤 패턴"
date: 2021-05-25
categories: java static eclipse
---

## static
#### ✔ static 변수
- **클래스 변수** 라고도 한다.
- 모든 객체(인스턴스)에 공통인 변수가 필요할 때 사용한다.  
  *(ex. 자동차의 시리얼 번호, 학생번호)*
- static 변수는 하나의 클래스에 하나만 존재한다.
- `클래스명.static변수명` *(ex. Car.numberOfCars)*  으로 사용한다.

#### ✔ static 메소드
- **클래스 메소드** 라고도한다
- 객체가 생성되지 않은 상태에서 호출되는 메소드이므로 객체 안에서만 존재하는 인스턴스변수(멤버변수)는 사용할 수 없고, **정적변수(static 변수)와 지역변수(로컬변수)만 사용할 수 있다.**
- 인스턴스변수(멤버변수)는 객체가 생성되어야만 사용할 수 있기 때문이다.
- `클래스명.static메소드명` *(ex. Math.sqrt(9.0);)* 으로 호출한다.

## singleton

#### ✔ 싱글톤패턴
- 싱글톤패턴은 인스턴스를 단 하나만 생성하는 디자인 패턴이다. *(ex. 회사 객체 - 직원은 여러명이지만 회사는 하나이다.)*
- 싱글톤 패턴 특징
  - **private 생성자**  
    단 하나의 인스턴스를 보장하기 위해
  - **static으로 선언된 인스턴스**  
    프로그램 전체에서 사용할 유일한 인스턴스  
    private로 선언하여 외부에서 접근하지 못하도록 제한한다.
  - **외부에서 참조할 수 있는 public 메소드**  
    오직 getInstance() 메소드로 instance에 접근가능하다.


##### Eager initialization (이른 초기화 방식)
```java
package singleton;

// 싱글톤 패턴으로 회사 클래스 구현하기
public class Company {
	// private static으로 생성
	private static Company instance = new Company();

	// private 생성자
	private Company() {}

	// private로 선언한 인스턴스 반환하는 public메소드
	public static Company getInstance() {
		return instance;
	}
}
```
장점 : Thread-safe하다.  
단점 : 사용유무와 상관없이 항상 메모리에 상주한다.


##### Lazy initialization (늦은 초기화 방식)
```java
package singleton;

// 싱글톤 패턴으로 회사 클래스 구현하기
public class Company {
	// 미리 생성안함
	private static Company instance;

	// private 생성자
	private Company() {}

	// private로 선언한 인스턴스 반환하는 public메소드
	public static Company getInstance() {
    if (instance == null) { // 최초로 사용할 때 인스턴스 생성
      instance = new Company();
    }
		return instance;
	}
}
```
장점 : 객체가 필요할 때 getInstance()메소드로 인스턴스를 얻을 수 있다.
단점 : 단 하나의 instance 보장 문제 - multi-thread 환경에서 여러 인스턴스가 생성될 수 있다.


---
##### signletonMain
```java
package singleton;

// 싱글톤 패턴으로 회사 클래스 구현하기

public class Company {

	// 유일한 인스턴스 - Company클래스 내부에서 하나의 인스턴스 생성
	// private로 외부에서 이 인스턴스에 접근하지 못하도록 제한
//	private static Company instance = new Company();
//	private static Company instance = null;
	private static Company instance;
//	- 미리 생성하지 않는다


	// 생성자 private로 지정
	// - 디폴트 생성자 자동으로 만들어지지 않고, Company클래스 내부에서만 이 클래스의 생성을 제어할 수 있다.
	private Company() {
		System.out.println("Create Instance");
	}

	// private로 선언한 인스턴스 반환하는 public메소드
	// static메소드 - 인스턴스 생성과 상관없이 호출해야함
	// 유일하게 instance에 접근할 수 있는 방법
	public static Company getInstance() {
		// 최초로 사용할 때 인스턴스 생성
		if (instance == null) {
			instance = new Company();
		}
		return instance;
	}


	public static void main(String[] args) {
		Company myCompany1 = Company.getInstance();
		// 최초로 인스턴스 생성 - "Create Instance"출력
		Company myCompany2 = Company.getInstance();
		// 인스턴스 생성안됨 반환만 함

		System.out.println(myCompany1 == myCompany2);
		// true 출력
	}
}

```


###### 참고
https://ssoonidev.tistory.com/98  
https://limkydev.tistory.com/67 
박은종, 『Do it! 자바 프로그래밍 입문』, 이지퍼블리싱(2020), p193-196

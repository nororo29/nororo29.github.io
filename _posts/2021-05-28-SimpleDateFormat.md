---
title: "[Java] 날짜와 SimpleDateFormat 클래스"
date: 2021-05-28
categories: java SimpleDateFormat eclipse
---

## ✔ SimpleDateFormat 클래스
##### 생성자
- `SimpleDateFormat()`
- `SimpleDateFormat(String pattern)`

##### 메서드
- `public final String format(Date date)`  
  Date형 자료를 받아 format된 값을 **String형으로 반환** 한다.
- `public Date parse(String source) throws ParseException`  
  String형 자료를 받아 parse(파싱/해석)된 **Date형을 반환** 한다.

##### 사용방법
- 원하는 출력형식의 패턴을 매개변수로 하여 SimpleDateFormat을 생성한다.
```java
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");
SimpleDateFormat df = new SimpleDateFormat("yyyy년 MM월 dd일");
```
- 출력하려는 Date 인스턴스를 format() 메서드를 통해 String값으로 받은 후 출력한다.
```java
Date today = new Date();  // 오늘 날짜
SimpleDateFormat df = new SimpleDateFormat("yyyy년 MM월 dd일");

String result = df.format(today);

System.out.println(result); // 2021년 05월 28일
```

- parse() 메서드를 통해 String값으로 된 날짜를 Date형으로 받는다.
```java
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");  

Date d;
d = df.parse("2018-5-13");

System.out.println(d);  // Sun May 13 00:00:00 KST 2018
System.out.println(df.format(d)); // 2018-05-13  // format()메소드를 통해 패턴에 맞게 변환
```
- parse()를 쓸 경우 `throws ParseException` 필요

---
##### class에서 Date 멤버변수 사용하기

```Java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

// 주문정보 클래스
public class orderForm {
	double orderId;
	String clientId;
	Date date;
	String clientName;
	String orderNum;
	String address;

	public void printOrder() {  // 출력 메서드
		SimpleDateFormat df = new SimpleDateFormat("yyyy년 MM월 dd일");

		System.out.printf("주문 번호 : %.0f\n", orderId);
		System.out.println("주문자 아이디 : " + clientId);
		System.out.println("주문 날짜 : " + df.format(date));
		System.out.println("주문자 이름 : " + clientName);
		System.out.println("주문 상품 번호 : " + orderNum);
		System.out.println("배송 주소 : " + address);
	}

  //======================main=======================
	public static void main(String[] args) throws ParseException {

		orderForm o1 = new orderForm();
		SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");

		o1.orderId = 201803120001d;
		o1.clientId = "abc123";
		o1.date = df.parse("2018-10-3");
		o1.clientName = "홍길순";
		o1.orderNum = "PD0345-12";
		o1.address = "서울시 영등포구 여의도동 20번지";

		o1.printOrder();
	}
}

```  
```
//출력결과
주문 번호 : 201803120001
주문자 아이디 : abc123
주문 날짜 : 2018년 10월 03일
주문자 이름 : 홍길순
주문 상품 번호 : PD0345-12
배송 주소 : 서울시 영등포구 여의도동 20번지
```


###### 참고
https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/text/DateFormat.html#format(java.lang.Object,java.lang.StringBuffer,java.text.FieldPosition)  
https://ryan-han.com/post/java/java-calendar-date/

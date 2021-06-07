---
title: "[Java] DVD 대여 서비스"
date: 2021-06-06
categories: java eclipse
---

## ✔ DVD 대여 서비스
![3](https://user-images.githubusercontent.com/50298349/121013277-95c9f680-c7d3-11eb-87dd-cea1751bec06.jpg)

##### 기능
- 신규가입(이름, 전화번호) : Customer(이름, id, 전화번호) 객체 생성 => customerList에 저장됨
- 고객검색(id) : id입력하면 => 이름, 전화번호 출력
- dvd 등록(ISBN, 제목, 장르) : dvd 객체 생성 => dvdList에 저장됨
- dvd 검색(ISBN) : 제목, 장르, 대출가능여부 출력
- dvd 대여(ISBN, 고객id) : 대여가능여부 확인 => 대여 고객 id와 날짜 저장
- dvd 반납(ISBN) : 대여가능여부 true로,,
- 대여 전체 조회(대여일) : 대여일에 맞는 대여 고객 전체 조회
- 고객 대여dvd 전체 조회(id) : 전체 조회


#### Customer

```Java
import java.util.ArrayList;

public class Customer {
	private static int serialNum = 1000;
	private String name;
	private int id;
	private String phoneNum;
	private ArrayList<Dvd> rentalDvd;  // 고객별 빌린 DVD 리스트

	public Customer(String name, String phoneNum) {
		serialNum++;
		this.name = name;
		this.phoneNum = phoneNum;
		this.id = serialNum;
		rentalDvd = new ArrayList<Dvd>(5);  // 최대 5개 빌리는 것 가능
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getPhoneNum() {
		return phoneNum;
	}

	public void setPhoneNum(String phoneNum) {
		this.phoneNum = phoneNum;
	}

	public ArrayList<Dvd> getRentalDvd() {
		return rentalDvd;
	}

	public void setRentalDvd(ArrayList<Dvd> rentalDvd) {
		this.rentalDvd = rentalDvd;
	}
}
```

#### Dvd

```Java
public class Dvd {
	private String ISBN;
	private String name;
	private String genre;
	private String date;  // 빌린 날짜
	private boolean canRent;  // 대여 가능 여부
	private int customerId;  // 빌려간 고객 id

	public Dvd(String isbn, String name, String genre) {
		this.ISBN = isbn;
		this.name = name;
		this.genre = genre;
		this.canRent = true;
		this.date = "";
	}

	public String getISBN() {
		return ISBN;
	}

	public void setISBN(String iSBN) {
		ISBN = iSBN;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getGenre() {
		return genre;
	}

	public void setGenre(String genre) {
		this.genre = genre;
	}

	public String getDate() {
		return date;
	}

	public void setDate(String date) {
		this.date = date;
	}

	public boolean isCanRent() {
		return canRent;
	}

	public void setCanRent(boolean canRent) {
		this.canRent = canRent;
	}

	public int getCustomerId() {
		return customerId;
	}

	public void setCustomerId(int customerId) {
		this.customerId = customerId;
	}


}
```

#### DvdLibrary

```Java
class DvdLibrary {
	private ArrayList<Dvd> dvdList;  // 전체 dvd리스트
	private ArrayList<Customer> customerList;  // 전체 고객 리스트

	public DvdLibrary() {
		dvdList = new ArrayList<>();
		customerList = new ArrayList<Customer>();
	}

	public ArrayList<Dvd> getDvdList() {
		return dvdList;
	}

	public void setDvdList(ArrayList<Dvd> dvdList) {
		this.dvdList = dvdList;
	}

	public ArrayList<Customer> getCustomerList() {
		return customerList;
	}

	public void setCustomerList(ArrayList<Customer> customerList) {
		this.customerList = customerList;
	}

	// 신규가입
	public void addCustomer(String name, String phoneNum) {
		Customer c = new Customer(name, phoneNum);
		customerList.add(c);
	}

	// 고객검색
	public Customer searchCustomer(int id) {
		Iterator<Customer> ir = customerList.iterator();
		while (ir.hasNext()) {
			Customer c = ir.next();
			if (c.getId() == id) {
				System.out.printf("이름:%s  \tid:%d  \t번호:%s\n", c.getName(), c.getId(), c.getPhoneNum());
				return c;
			}
		}
		System.out.println("id:" + id + " 존재하지 않습니다.");
		return null;
	}

	// dvd 등록
	public void addDvd(String isbn, String name, String genre) {
		Dvd d = new Dvd(isbn, name, genre);
		dvdList.add(d);
	}

	// dvd 검색
	public Dvd searchDvd(String isbn) {
		Iterator<Dvd> ir = dvdList.iterator();
		while (ir.hasNext()) {
			Dvd d = ir.next();
			if (d.getISBN().equals(isbn)) {
				System.out.printf("제목:%s  \t장르:%s  \t대출가능:%b\n", d.getName(), d.getGenre(), d.isCanRent());
				return d;
			}
		}
		System.out.println("ISBN:" + isbn + " 존재하지 않습니다.");
		return null;
	}

	// dvd 대여
	public void rentDvd(String isbn, int id) {

		Dvd d = searchDvd(isbn);
		Customer c = searchCustomer(id);

		if (d == null || c == null)
			return;

		if (c.getRentalDvd().size() >= 5) {
			System.out.println("대출 불가합니다.");
			System.out.println("1인당 5권 대출가능");
			return;
		}

		if (d.isCanRent()) {
			Date today = new Date();
			DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
			String todayStr = df.format(today);

			c.getRentalDvd().add(d);
			d.setDate(todayStr);
			d.setCanRent(false);
			d.setCustomerId(id);
			System.out.println("대출하였습니다.");
		} else {
			System.out.println("대출 불가 항목입니다.");
		}
	}

	// dvd 반납
	public void returnDvd(String isbn) {
		Dvd d = searchDvd(isbn);
		Customer c = searchCustomer(d.getCustomerId());

		if (d == null || c == null)
			return;

		d.setCanRent(true);
		d.setCustomerId(-1);
		c.getRentalDvd().remove(d);
		System.out.println("반납하였습니다.");
	}

	// 대여일 대여 전체 조회
	public void rentStatus(String date) {
		Iterator<Dvd> ir = dvdList.iterator();
		while (ir.hasNext()) {
			Dvd d = ir.next();
			if (d.getDate().equals(date)) {
				System.out.printf("대여목록:%s	  \t대여자:%d\n", d.getName(), d.getCustomerId());
			}
		}
	}

	// 고객 대여dvd 조회
	public void customerRentStatus(int id) {
		Iterator<Customer> ir = customerList.iterator();
		while (ir.hasNext()) {
			Customer c = ir.next();
			if (c.getId() == id) {
				ArrayList<Dvd> dvdArr = c.getRentalDvd();
				Iterator<Dvd> ir2 = dvdArr.iterator();
				while (ir2.hasNext()) {
					Dvd d = ir2.next();
					searchDvd(d.getISBN());
				}
			}
		}
	}
}
```

#### DvdRentalTest

```Java
public class DvdRentalTest {

	public static void main(String[] args) {

		DvdLibrary dl = new DvdLibrary();

		// 신규가입 (이름, 전화번호) // (이름, id, 전화번호)Customer 객체 생성 -> customerList에 저장됨
		dl.addCustomer("lucy", "010-0000-0000"); // 1001번 부터 생성
		dl.addCustomer("gina", "010-1111-0000"); // 1002
		dl.addCustomer("tom", "010-2222-0000"); // 1003

		// 고객검색 (id) // id입력하면 -> 이름, 전화번호 출력
		System.out.println("===== 고객검색 =====");
		dl.searchCustomer(1001);
		dl.searchCustomer(1002);
		dl.searchCustomer(1003);

		// dvd 등록 (ISBN, 제목, 장르) // dvd 객체 생성 => dvdList에 저장됨
		dl.addDvd("a-1000", "movie1", "music");
		dl.addDvd("a-1001", "movie2", "action");
		dl.addDvd("a-1002", "movie3", "fun");
		dl.addDvd("b-1000", "movie4", "music");
		dl.addDvd("b-1001", "movie5", "action");
		dl.addDvd("b-1002", "movie6", "action");

		// dvd 검색 (ISBN) // 제목, 장르, 대출가능여부 출력
		System.out.println();
		System.out.println("===== dvd검색 =====");
		dl.searchDvd("a-1000");
		dl.searchDvd("a-1001");
		dl.searchDvd("a-1002");
		dl.searchDvd("b-1000");
		dl.searchDvd("b-1001");
		dl.searchDvd("b-1002");

		// dvd 대여 (ISBN, 고객id) //대여가능여부 확인 -> 대여 고객 id와 날짜 입력
		System.out.println();
		System.out.println("===== dvd 대여 =====");
		dl.rentDvd("a-1000", 1001);
		System.out.println();
		dl.rentDvd("a-1001", 1001);
		System.out.println();
		dl.rentDvd("a-1000", 1002); // 대출불가
		System.out.println();
		dl.rentDvd("a-1002", 1001);
		System.out.println();
		dl.rentDvd("a-1002", 1003); // 대출불가
		System.out.println();
		dl.rentDvd("b-1000", 1001);
		System.out.println();
		dl.rentDvd("b-1002", 1001);
		System.out.println();
		dl.rentDvd("b-1001", 1001);  // 대출 불가 - 1인당 5권만 가능

		// dvd 반납 (ISBN) //대여가능여부 가능으로,,
		System.out.println();
		System.out.println("===== dvd 반납 =====");
		dl.returnDvd("a-1000");
		System.out.println();
		dl.returnDvd("b-1000");

		// 대여 전체 조회 (대여일) //대여일에 맞는 대여 고객 전체 조회
		System.out.println();
		System.out.println("===== 대여조회 =====");
//		dl.getDvdList().forEach(s -> System.out.println(s.getDate()));
		Date today = new Date();
		DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
		String date = df.format(today);
		dl.rentStatus(date);

		// 고객 대여dvd 전체 조회 (id) //전체 조회
		System.out.println();
		System.out.println("===== 고객 대여 정보 =====");
		System.out.println("-- id:1001 --");
		dl.customerRentStatus(1001);
		System.out.println("-- id:1002 --");
		dl.customerRentStatus(1002);
		System.out.println("-- id:1003 --");
		dl.customerRentStatus(1003);
	}

}

```

```
출력---
===== 고객검색 =====
이름:lucy  	id:1001  	번호:010-0000-0000
이름:gina  	id:1002  	번호:010-1111-0000
이름:tom  	id:1003  	번호:010-2222-0000

===== dvd검색 =====
제목:movie1  	장르:music  	대출가능:true
제목:movie2  	장르:action  	대출가능:true
제목:movie3  	장르:fun  	대출가능:true
제목:movie4  	장르:music  	대출가능:true
제목:movie5  	장르:action  	대출가능:true
제목:movie6  	장르:action  	대출가능:true

===== dvd 대여 =====
제목:movie1  	장르:music  	대출가능:true
이름:lucy  	id:1001  	번호:010-0000-0000
대출하였습니다.

제목:movie2  	장르:action  	대출가능:true
이름:lucy  	id:1001  	번호:010-0000-0000
대출하였습니다.

제목:movie1  	장르:music  	대출가능:false
이름:gina  	id:1002  	번호:010-1111-0000
대출 불가 항목입니다.

제목:movie3  	장르:fun  	대출가능:true
이름:lucy  	id:1001  	번호:010-0000-0000
대출하였습니다.

제목:movie3  	장르:fun  	대출가능:false
이름:tom  	id:1003  	번호:010-2222-0000
대출 불가 항목입니다.

제목:movie4  	장르:music  	대출가능:true
이름:lucy  	id:1001  	번호:010-0000-0000
대출하였습니다.

제목:movie6  	장르:action  	대출가능:true
이름:lucy  	id:1001  	번호:010-0000-0000
대출하였습니다.

제목:movie5  	장르:action  	대출가능:true
이름:lucy  	id:1001  	번호:010-0000-0000
대출 불가합니다.
1인당 5권 대출가능

===== dvd 반납 =====
제목:movie1  	장르:music  	대출가능:false
이름:lucy  	id:1001  	번호:010-0000-0000
반납하였습니다.

제목:movie4  	장르:music  	대출가능:false
이름:lucy  	id:1001  	번호:010-0000-0000
반납하였습니다.

===== 대여조회 =====
대여목록:movie1	  	대여자:-1
대여목록:movie2	  	대여자:1001
대여목록:movie3	  	대여자:1001
대여목록:movie4	  	대여자:-1
대여목록:movie6	  	대여자:1001

===== 고객 대여 정보 =====
-- id:1001 --
제목:movie2  	장르:action  	대출가능:false
제목:movie3  	장르:fun  	대출가능:false
제목:movie6  	장르:action  	대출가능:false
-- id:1002 --
-- id:1003 --
```

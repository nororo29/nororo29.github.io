---
title: "[Java] 전화번호부"
date: 2021-06-06
categories: java eclipse
---

## ✔ 전화번호부
이미지...

##### 기능
- 입력(이름, 번호) -> 객체 생성 , 전화번호부에 저장(put)
- 전체출력() -> 전체정보 출력
- 검색(이름 or 번호) -> 없는 정보입니다 or 이름, 번호 출력
- 삭제(이름 or 번호) -> 없는 정보입니다 or 삭제(remove)
- 변경(이름 or 번호) -> 없는 정보입니다 or 새로운번호입력하세요 -> 번호 업데이트
- 종료() -> 종료문 출력…


#### Phone

```Java
class Phone {
	private String name;
	private String num;

	public Phone(String name, String num) {
		this.name = name;
		this.num = num;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getNum() {
		return num;
	}

	public void setNum(String num) {
		this.num = num;
	}

	@Override
	public String toString() {
		return name;
	}

	public String print() {
		return name + ":" + num;
	}

}
```

#### PhoneBook

```Java
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

class PhoneBook {
	Map<String, Phone> phonebook;

	public PhoneBook() {
		phonebook = new HashMap<String, Phone>();
	}

	// 입력(이름, 번호) -> 객체생성, 전화번호부에 저장
	public void putNumber(String name, String num) {
		Phone phone = new Phone(name, num);
		phonebook.put(num, phone);
	}

	// 전체출력() -> 전체객체 출력
	public void showAllNumber() {
		System.out.println("===== 전체 전화번호 =====");
		System.out.println(phonebook);
	}

	// 검색(이름 or 번호) -> 없는정보입니다 or 이름,번호 출력
	public void searchPhone(String n) {
		for (Phone p : phonebook.values()) {
			if (p.getName().equals(n) || p.getNum().equals(n)) { // 이름이 같거나 번호가 같으면 출력
				System.out.println(p.print());
				return;
			}
		}
		System.out.println("없는 정보입니다.");
	}

	// 삭제(이름 or 번호) -> 삭제
	public void deletePhone(String n) {
		for (Phone p : phonebook.values()) {
			if (p.getName().equals(n) || p.getNum().equals(n)) {
				System.out.println(phonebook.get(p.getNum()) + "님의 번호가 삭제되었습니다.");
				phonebook.remove(p.getNum());
				return;
			}
		}
		System.out.println("없는 정보입니다.");
	}

	// 변경(이름 or 번호) -> 없는 이름입니다 or 새로운번호를 입력하시오 -> 번호 바꿈
	public void updatePhone(String n) {
		for (Phone p : phonebook.values()) {
			if (p.getName().equals(n) || p.getNum().equals(n)) {
				Scanner sc = new Scanner(System.in);

				System.out.print("새로운 번호를 입력하시오 : ");
				String newNum = sc.nextLine();

				// 새로입력한 번호가 숫자로 이뤄졌는지 검사
				try {
					Double.parseDouble(newNum);
				} catch (NumberFormatException e) {
					System.out.println("번호오류");
					sc.close();
					return;
				}

				// 새로입력한번호를 key로, 예전 Phone객체를 value로 하고 객체 삭제
				phonebook.put(newNum, phonebook.remove(p.getNum()));
				p.setNum(newNum); // 예전 Phone객체 번호 변경

				System.out.println(phonebook.get(p.getNum()) + "님의 번호가 변경되었습니다.");
				sc.close();
				return;
			}
		}
		System.out.println("없는 정보입니다.");
	}

	// 종료() -> 종료문 출력...
	public void exit() {
		System.out.println("종료되었습니다.");
	}

}
```

#### PhoneBookTest

```Java
public class PhoneBookTest {

	public static void main(String[] args) {
//		전화번호 관리 프로그램
		PhoneBook pb = new PhoneBook();

//		전화번호 입력
		// (이름, 번호) -> 객체생성, 전화번호부에 저장
		pb.putNumber("lucy", "01011111111");
		pb.putNumber("gina", "01011112222");
		pb.putNumber("tom", "01011113333");

//		전체 정보 출력
		// () -> 전화번호부에 저장된 객체들 출력
		pb.showAllNumber();

//		전화번호 검색
		// (이름) -> 없는 정보입니다 or 이름,번호 출력
		// (전화번호) -> 없는 정보입니다 or 이름,번호 출력
		System.out.println("===== 검색 =====");
		pb.searchPhone("01011111111");
		pb.searchPhone("gina");

//		전화번호 삭제
		// (이름) -> 전화번호부에서 삭제
		// (번호) -> 전화번호부에서 삭제
		System.out.println("===== 삭제 =====");
		pb.deletePhone("lucy");
		pb.deletePhone("01011111111");
		pb.showAllNumber();

//		전화번호 변경
		// (이름) -> 없는 이름입니다 or 새로운번호를 입력하시오 -> 번호 바꿈
		// (번호) -> 없는 번호입니다 or 새로운번호를 입력하시오 -> 번호 바꿈
		System.out.println("===== 변경 =====");
		pb.updatePhone("lucy");
		pb.updatePhone("01011112222");
		pb.showAllNumber();

//		종료?
		// exit() -> 종료문 출력...
		pb.exit();
	}

}
```

```
출력---
===== 전체 전화번호 =====
{01011112222=gina, 01011113333=tom, 01011111111=lucy}
===== 검색 =====
lucy:01011111111
gina:01011112222
===== 삭제 =====
lucy님의 번호가 삭제되었습니다.
없는 정보입니다.
===== 전체 전화번호 =====
{01011112222=gina, 01011113333=tom}
===== 변경 =====
없는 정보입니다.
새로운 번호를 입력하시오 : 01099999999
gina님의 번호가 변경되었습니다.
===== 전체 전화번호 =====
{01099999999=gina, 01011113333=tom}
종료되었습니다.
```

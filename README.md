# 책 관리 프로그램
학원에서 조건문과 반복문 등 이해를 하기 위한 미니 프로젝트
<br>
책을 대여하거나, 반납, 리스트를 볼 수 있습니다.

# 목차
* 프로젝트 개요
* 프로젝트 기능 설명
* 프로젝트 후기

<br>

# 프로젝트 개요
* 프로젝트 이름 : 책 관리 프로그램
* 프로젝트 개발 기간 : 2023.12 ~ 2023.12(7일)
* 프로젝트 구성 인원 : 1명(개인 프로젝트)
* 프로젝트 개발 환경
  * 언어 : JAVA


# 프로젝트 기능 설명

## Exam01_Book / 책 정보
* 책을 관리하는 클래스이며, 책 추가, 삭제, 정보 조회, 책 대여 검사를 담당합니다.

<br>

<details>
    <summary>코드 보기</summary>

```java
public class Exam01_Book {

	String title; // 책 제목
	String add_book; // 책을 추가할 book 정보를 저장
	String remove_book; // 책을 삭제할 book 정보를 저장
	boolean book_isrental; // 책 대여 여부
	StringBuilder book_data; // 책 데이터 저장
	Exam01_Book_Rental book_rental; // 책 대여 정보 저장

	List<Exam01_Book> books = new ArrayList<Exam01_Book>(); // 책 리스트

	public Exam01_Book() {

//		보유 서적 : 예제로 배우는 Java, HTML 웹페이지 만들기, 슬램덩크, 자료구조
		books.add(new Exam01_Book("예제로 배우는 Java", false));
		books.add(new Exam01_Book("HTML 웹페이지 만들기", false));
		books.add(new Exam01_Book("슬램덩크", false));
		books.add(new Exam01_Book("자료구조", false));
	};

	public Exam01_Book(String title, boolean isrental) {
		this.title = title;
	}
	
	public String getTitle() {
		return title;
	}
	
//	새로운 책을 등록 / 기존의 있던 Book을 검사
	public StringBuilder BookAdd(String book1) {
		StringBuilder ab = new StringBuilder();
		add_book = book1; // 추가할 책을 저장

		boolean isbook = false;

		// 현재 저장된 리스트 중에 갖고 있는 책이 있다면 isbook true
		for (int i = 0; i < books.size(); i++) {
			if ((books.get(i).getTitle().equals(book1))) {
				isbook = true; // 책이 이미 존재한다면 true
				break;
			}
		}
		if (!isbook) {
			// 책이 존재하지 않은 책이라면 추가하기
			books.add(new Exam01_Book(book1, isbook));
			book_data = ab.append("== 책 추가 ==\n\n");
			add_book = null;
			return ab;
		}
		return ab;
	}

	// 등록된 책을 삭제
	public void BookrRemove(String book1) {
		remove_book = book1;
		StringBuilder ab = new StringBuilder();

		// 리스트 중에 해당 책을 찾아서 삭제
		for (int i = 0; i < books.size(); i++) {
			if (books.get(i).getTitle().equals(book1)) {
				books.remove(i); // 삭제
				book_data = ab.append("== 책 삭제 ==\n\n");
				break;
			}
		}
	}

	@Override
	// 책 정보를 출력
	public String toString() { // 책을 출력
		StringBuilder ab = new StringBuilder();

		// 책 대여 정보를 초기화
		book_rental = new Exam01_Book_Rental(book_rental.getBookName(), book_rental.getIsRental());
		
		
//		책 각 추가 및 삭제에 대한 정보가 없으면 기존 정보 출력
		if ((add_book == null && remove_book == null) && book_rental.getBookName() == null) {
			ab.append(bookcontents());
			ab.append(bIsrental() + "\n");
		} else if ((add_book != null && remove_book == null)) {
			ab.append(book_data); // 책추가
			ab.append(bookcontents());
			ab.append(bIsrental() + "\n");
			add_book = null;

		} else if (add_book == null && remove_book != null) {
			ab.append(book_data); // 책 삭제
			ab.append(bookcontents());
			ab.append(bIsrental() + "\n");
			remove_book = null;
		}
		// 책 대여 여부 확인
		if (book_rental.getIsRental()) {
			ab.append(bookcontents());
			ab.append(book_rental.getBookName() + " 책을 대여 완료하였습니다.\n");
			ab.append("대여기간 : " + book_rental.getToday() + "일 입니다.\n");
			ab.append(bIsrental() + "\n");
			
		}

		return ab.toString();
	}
	
	// 카페 정보를 출력하는 메서드
	public String bookcontents() {
		
		StringBuilder content = new StringBuilder();
		
		content.append("== 카페 정보 ==\n");
		content.append(String.format("이름 : %s\n", "IT 카페")); // 카페이름
		content.append(String.format("주소 : %s\n", "신림역 3번 출구")); // 카페 주소
		content.append("보유 서적 : \n");
		
		return content.toString();
	}

	// 책 대여 가능 여부 메서드
	public String bIsrental() {
		
		StringBuilder ab = new StringBuilder();

		// 책 리스트 중에 책 대여 가능 검사
			for(int i=0; i < books.size(); i++) {
				StringBuilder bookinfo = new StringBuilder();
				if (books.get(i).book_isrental) {
					bookinfo.append(String.format("%s : %s\n", books.get(i).getTitle(), "대여 불가\n" ));
				} else {
					bookinfo.append(String.format("%s : %s\n", books.get(i).getTitle(), "대여 가능"));
				}
				ab.append(bookinfo);
			}
			return ab.toString();
	}

	@Override
	public boolean equals(Object o) {
		// 비교 코드
		if (this == o) {
			return true;
		}

		if (o == null || getClass() != o.getClass()) {
			return false;
		}

		Exam01_Book book = (Exam01_Book) o;
		return Objects.equals(getTitle(), book.getTitle());
	}

	@Override
	public int hashCode() {
		return Objects.hash(title);
	}

}

```
</details>
<br>

## Exam01_Book_Rental / 대여 정보
* 책 대여를 담당하는 클래스이며, 대여 기간 7일입니다.
<br>

<details>
    <summary>코드 보기</summary>

```java
// Rental 클래스는 Book 클래스를 상속하여 Book의 클래스 메소드 
public class Exam01_Book_Rental extends Exam01_Book  {

//	책, 대여, 반납

	boolean isrental = false; // 책 대여 여부
	private static int isInt_rantal = 7; // 대여 기간 7일 고정
	String book_name; // 책 이름
	Date book_return_date; // 반납 예정일

	Exam01_Book Book_lists;
//	반납기간은 항시 일수 해당 지점에 12시
	LocalDate today;

	public Exam01_Book_Rental() {
	};

	public Exam01_Book_Rental(String book_name, boolean isrental) {
		super(book_name, isrental);
		this.book_name = book_name;
		this.isrental = isrental;
	}

	public String getBookName() {
		return book_name;
	}

	public boolean getIsRental() {
		return isrental;
	}

	public int getIsIntRantal() {
		return isInt_rantal;
	}
	
	public void setBookName(String book_name) {
		this.book_name = book_name;
	}
	
	public void setIsRental(boolean isrental) {
		this.isrental = isrental;
	}
	
	public LocalDate getToday() {
		today = LocalDate.now();
		LocalDate returntoday = today.plusDays(7);
		
		return returntoday;
	}

	public void BookIsRental(String bookname) {
//		책 대여가 가능한지 여부확인
		boolean isbook = false;
		
		// 현재 날짜 출력
		today = LocalDate.now(); 
		
//		7일 후 날짜 출력
		LocalDate returntoday = today.plusDays(7);
		
		DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
		 String formattoday = returntoday.format(formatter);
		 
		for(int i=0; i < books.size(); i++) {
//		for (Exam01_Book book : books) {
			StringBuilder ab = new StringBuilder();
//			대여할 책이 기존 책에 있는지
				if (books.get(i).getTitle().equals(bookname)) {
					isbook = true;
//				해당 책이 대여한게 있는지
					if (books.get(i).book_isrental) {
						JOptionPane.showMessageDialog(null, "이미 대여된 " + books.get(i).getTitle() + "책입니다.", "북 카페",
								JOptionPane.PLAIN_MESSAGE);
//					책이 있고 대여한게 없다면 대여 가능
					} else {
						JOptionPane.showMessageDialog(null, "해당" + books.get(i).getTitle() + " 대여가 가능합니다.",
								"북 카페", JOptionPane.PLAIN_MESSAGE);
						books.get(i).book_isrental = true; // 해당 책은 대여가 됬다.
						book_rental.setBookName(bookname);
						book_rental.setIsRental(books.get(i).book_isrental);
					}
					break;
				}
		}
	}

}

```
</details>

<br>
<br>

* 책 대여 결과
![책 관리시스템_01](https://github.com/koyuhjkl123/Book-cafe/assets/94844952/30dc4769-7863-445e-8750-f0af5ec9aa38)
<br>

![책 관리시스템_02](https://github.com/koyuhjkl123/Book-cafe/assets/94844952/f74548b3-5671-43d2-934b-abbf5b1197d5)

<br>

* 기존에 있는 재고중에 대여 가능한지 확인 후 "JOptionPane.showMessageDialog" 활용하여 메세지를 출력하여 대여 가능 여부를 보냅니다.
<details>
    <summary>코드 보기</summary>

```java
for(int i=0; i < books.size(); i++) {
			StringBuilder ab = new StringBuilder();
//			대여할 책이 기존 책에 있는지
				if (books.get(i).getTitle().equals(bookname)) {
					isbook = true;
//				해당 책이 대여한게 있는지
					if (books.get(i).book_isrental) {
						JOptionPane.showMessageDialog(null, "이미 대여된 " + books.get(i).getTitle() + "책입니다.", "북 카페",
								JOptionPane.PLAIN_MESSAGE);
//					책이 있고 대여한게 없다면 대여 가능
					} else {
						JOptionPane.showMessageDialog(null, "해당" + books.get(i).getTitle() + " 대여가 가능합니다.",
								"북 카페", JOptionPane.PLAIN_MESSAGE);
						books.get(i).book_isrental = true; // 해당 책은 대여가 됬다.
						book_rental.setBookName(bookname);
						book_rental.setIsRental(books.get(i).book_isrental);
					}
					break;
				}
		}
```
</details>

![책 관리시스템_03](https://github.com/koyuhjkl123/Book-cafe/assets/94844952/39c69d55-6e83-45f8-91ac-e684508c621b)


## Exam01_Drink / 음료 정보

## Exam01_BookTest // 실행 클래스


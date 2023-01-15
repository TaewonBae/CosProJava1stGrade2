# COS PRO Java 1급 - 모의고사1

### 1차) 문제 1 - 도서 대여점 운영
```Java
class Main {
    interface Book{
        public int getRentalPrice(int day);
    }
    
    class ComicBook implements Book {
			//인터페이스에서 구현된 메소드를 재정의 해줘야함
        public  int getRentalPrice(int day){
            int cost = 500; //기본요금
            day -= 2; //이후 추가되는 날짜 수 계산
            if(day > 0) // 추가되는 날이 있으면
                cost += day*200;
            return cost;
        }
    }
    
    class Novel implements Book {
			//인터페이스에서 구현된 메소드를 재정의 해줘야함
        public  int getRentalPrice(int day){
            int cost = 1000;//기본요금
            day -= 3;//이후 추가되는 날짜 수 계산
            if(day > 0)// 추가되는 날이 있으면
                cost += day*300;
            return cost;
        }
    }

    public int solution(String[] bookTypes, int day) {
        Book[] books = new Book[50];
        int length = bookTypes.length;
        for(int i = 0; i < length; ++i){
            if(bookTypes[i].equals("comic"))
                books[i] = new ComicBook();
            else if(bookTypes[i].equals("novel"))
                books[i] = new Novel();   
        }
        int totalPrice = 0;
        for(int i = 0; i < length; ++i)
            totalPrice += books[i].getRentalPrice(day);
        return totalPrice;
    }
    // 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
    public static void main(String[] args) {
        Main sol = new Main();
        String[] bookTypes = {"comic", "comic", "novel"};
        int day = 4;
        int ret = sol.solution(bookTypes, day);

        // [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
        System.out.println("solution 메소드의 반환 값은 " + ret + " 입니다.");
    }
}
```

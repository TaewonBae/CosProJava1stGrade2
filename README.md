# COS PRO Java 1급 - 모의고사2

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

### 1차) 문제2  - 지하철 기다리기
```Java
class Main {
    //1. 00:00을 기준, 현재 시각을 분 단위로 변환(시*60+분)
    public int func_a(String times){
        int hour = Integer.parseInt(times.substring(0, 2));
        int minute = Integer.parseInt(times.substring(3));
        return hour*60 + minute;
    }
	
    public int solution(String[] subwayTimes, String currentTime) {
				
        int currentMinute = func_a(currentTime);
	//지하철 최소 대기 시간을 구하는 것이기 때문에 나올수 없는 큰 값을 초기화 값으로 주고
	//그것보다 작은 값을 넣어주려고 이렇게 초기화함
        int INF = 1000000000;
        int answer = INF;
	//subwayTimes의 i번 인덱스에 있는 값을 분으로 변경
        for(int i = 0; i < subwayTimes.length; ++i){
            int subwayMinute = func_a(subwayTimes[i]);
	    //지하철도착시각>=현재시각 경우에만 대기시간을 구해야됨 
	    // why? >> 이전에 지하철 도착시간은 이미 지나간 열차라 대기를 못함
            if(subwayMinute>=currentMinute){
                answer = subwayMinute - currentMinute;
                break;
            }
        }
	//오늘 탈 수 있는 지하철이 없으면 -1 return
        if(answer == INF)
            return -1;
        return answer;
    }

    // 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
    public static void main(String[] args) {
        Main sol = new Main();
        String[] subwayTimes1 = {"05:31", "11:59", "13:30", "23:32"};
        String currentTime1 = "12:00";
        int ret1 = sol.solution(subwayTimes1, currentTime1);

        // [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
        System.out.println("solution 메소드의 반환 값은 " + ret1 + " 입니다.");

        String[] subwayTimes2 = {"14:31", "15:31"};
        String currentTime2 = "15:31";
        int ret2 = sol.solution(subwayTimes2, currentTime2);

        // [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
        System.out.println("solution 메소드의 반환 값은 " + ret2 + " 입니다.");
    }
}
```

### 1차) 문제3  -  경품 당첨자 구하기
```Java
class Main {
    public int func_a(int n){
        int ret = 1;
        while(n > 0){
            ret *= 10;
            n--;
        }
        return ret;
    }

    int func_b(int n){
        int ret = 0;
        while(n > 0){
            ret++;
            n /= 10;
        }
        return ret;
    }
    
    int func_c(int n){
        int ret = 0;
        while(n > 0){
            ret += n%10;
            n /= 10;
        }
        return ret;
    }
    
	public int solution(int num) {
        int nextNum = num; // nextNum에 현재게시글의 넘버로 초기화
        while(true){
	    //1. 게시글 번호 1증가
            nextNum++;
	    //숫자의 자리수의 갯수 구하기
            int length = func_b(nextNum);
            if(length % 2 != 0)
                continue;//짝수가 아닌경우 다시while문의 처음으로 돌아가 짝수이면 2번으로
	    //2. 짝수이면 앞자리수와 뒷자리수 절반을 분리해야됨 그러려면 divisor를 구해야됨
            int divisor = func_a(length/2); //divisor : 10의 (자릿수 개수/2)제곱
            int front = nextNum / divisor;
            int back = nextNum % divisor;
            
	    //2-1. 앞 숫자 합, 뒷 숫자 합
            int frontSum = func_c(front);
            int backSum = func_c(back);
	    //2-2. 각 숫자의 합이 서로 같으면 3으로 while문을 빠져나감, 같지 않으면 1로 돌아감
            if(frontSum == backSum)
                break;
        }
	//3. 2에서 구한 당첨 번호 - 처음 매개변수로 주어진 게시글 번호 리턴
        return nextNum - num;
    }
    // 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
    public static void main(String[] args) {
        Main sol = new Main();
        int num1 = 1;
        int ret1 = sol.solution(num1);

        // [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
        System.out.println("solution 메소드의 반환 값은 " + ret1 + " 입니다.");

        int num2 = 235386;
        int ret2 = sol.solution(num2);

        // [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
        System.out.println("solution 메소드의 반환 값은 " + ret2 + " 입니다.");
    }
}
```

### 1차) 문제4  - 
```Java

```

### 1차) 문제5  - 
```Java

```

### 1차) 문제6  - 
```Java

```

### 1차) 문제7  - 
```Java

```

### 1차) 문제8  - 
```Java

```

### 1차) 문제9  - 
```Java

```

### 1차) 문제10  - 
```Java

```


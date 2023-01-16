# COS PRO Java 1급 - 모의고사2

### 2차) 문제1 - 도서 대여점 운영
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

### 2차) 문제2 - 지하철 기다리기
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

### 2차) 문제3 - 경품 당첨자 구하기
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

### 2차) 문제4 - 합이 k 배가 되는 수
```Java
// 다음과 같이 import를 사용할 수 있습니다.
import java.util.*;

class Main {
    public int solution(int[] arr, int K) {
        int answer = 0;
        int n = arr.length;//배열의 길이
			
        for(int i=0; i<n-2;i++){//index 0 ~ n-2 까지
		for(int j=i+1;j<n-1;j++){//index i+1 ~ n-1까지
			for(int k=j+1;k<n;k++){//index j+1 ~ n까지
				//3수의 합이 K의 배수이면 answer+=1;
				if((arr[i]+arr[j]+arr[k])%K==0){
					answer = answer+1;
					// System.out.println(arr[i]+" "+arr[j]+" "+arr[k]);
				}
			}
		}
	}
        return answer;
    }

    // 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
    public static void main(String[] args) {
        Main sol = new Main();
        int[] arr = {1, 2, 3, 4, 5};
        int K = 3;
        int ret = sol.solution(arr, K);


        // [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
        System.out.println("solution 메소드의 반환 값은 " + ret + " 입니다.");
    }
}
```

### 2차) 문제5 - 언제까지 오르막길이야(숫자가 연속해서 증가하는 가장 긴 구간의 길이 구하기)
```Java
import java.util.*;

class Main {
    public int solution(int[] arr) {
        int answer = 0;
	//1. 새로운 배열(dp)에 전부 1로 초기화
	int dp[] = new int[arr.length];
	for(int i=0; i<dp.length;i++){
		dp[i]=1;
	}
	//2. arr : 현재 인덱스 값 > 이전 인덱스 값
	//   dp : 이전 인덱스 값에 + 1
	for(int i=1; i<arr.length; i++){
		if(arr[i]>arr[i-1]){
			dp[i] = dp[i-1]+1;
			//{1,1,1,1,1,1,1,1,1,1,} >> {1,1,2,3,4,1,2,1,2,3}
		}
	}
	//3. dp에 들어있는 값 중 가장 큰 것
	for(int i=0; i<arr.length;i++){
		answer=Math.max(answer, dp[i]);
	}
        return answer;
    }
    // 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
    public static void main(String[] args) {
        Main sol = new Main();
        int[] arr = {3, 1, 2, 4, 5, 1, 2, 2, 3, 4};
        int ret = sol.solution(arr);

        // [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
        System.out.println("solution 메소드의 반환 값은 " + ret + " 입니다.");
    }
}

```

### 2차) 문제6 - 
```Java
// 다음과 같이 import를 사용할 수 있습니다.
import java.util.*;

class Main {
    public int[] solution(String commands) {
        // answer을 arr = {0,0} 좌표로 둔다.
        int[] answer = {0, 0};
	// 4가지 case로 나눠서 x좌표는 arr[0], y좌표는 arr[1]이동
	for(int i=0; i<commands.length();i++){
		if(commands.charAt(i)=='L'){
			answer[0]-=1;
		}else if(commands.charAt(i)=='R'){
			answer[0]+=1;
		}else if(commands.charAt(i)=='U'){
			answer[1]+=1;
		}else if(commands.charAt(i)=='D'){
			answer[1]-=1;
		}
	}
        return answer;
    }
    // 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
    public static void main(String[] args) {
        Main sol = new Main();
        String commands = "URDDL";
        int[] ret = sol.solution(commands);
				// L : (x,y) -> (x-1,y)
				// R : (x,y) -> (x+1,y)
				// U : (x,y) -> (x,y+1)
				// D : (x,y) -> (x,y-1)
        System.out.println("solution 메소드의 반환 값은 " + Arrays.toString(ret) + " 입니다.");
    }
}

```

### 2차) 문제7  - 거스름돈 구하기 
```Java
class Main {
    public int solution(int money) {
        int coin[] = {10, 50, 100, 500, 1000, 5000, 10000, 50000};
        int counter = 0;
        int idx = coin.length - 1;
        while (money > 0){
     	// 1. 해당 화폐 단위의 동전 개수를 구하여 합산.
	    counter += money/coin[idx]; // 총 거스름돈 동전의 개수 = 이전 동전 개수 + 금액/화폐단위(뒤에서부터 하나씩)
            money %= coin[idx]; // 2. 계산된 단위를 제외하고 남은 금액 계산 >> 남은 금액 : 금액%화폐단위
            idx -= 1; // 3. 다음 화폐 단위를 위해 인덱스 번호를 1 감소
        }
        return counter;
    }
    public static void main(String[] args) {
        Main sol = new Main();
        int money = 2760;
        int ret = sol.solution(money);

	System.out.println("solution 메소드의 반환 값은 " + ret + " 입니다.");
    }
}

```

### 2차) 문제8 - 규칙에 맞는 배열 구하기
```Java
import java.util.*;

class Main {
    public int[] solution(int[] arr) {
			
        int left = 0, right = arr.length - 1;
        int idx = 0;
        int[] answer = new int[arr.length];
        while(left <= right){
            if(idx % 2 == 0){
                answer[idx] = arr[left];
                left += 1;
            }
            else{
                answer[idx] = arr[right];
                right -= 1;
            }
            idx += 1;
        }
        return answer;
    }
    public static void main(String[] args) {
        Main sol = new Main();
        int[] arr = {1, 2, 3, 4, 5, 6};
        int[] ret = sol.solution(arr);

        System.out.println("solution 메소드의 반환 값은 " + Arrays.toString(ret) + " 입니다.");
    }
}
```

### 2차) 문제9  - 비밀번호 검사

```Java
class Main {
    public boolean solution(String password) {
			
        int length = password.length();
        for(int i = 0; i < length - 2; ++i){
            int firstCheck = password.charAt(i + 1) - password.charAt(i); // 다음꺼 - 처음꺼 체크
            int secondCheck = password.charAt(i + 2) - password.charAt(i+1); // 다다음꺼 - 다음꺼 체크
            if(firstCheck == secondCheck && (firstCheck == 1 || firstCheck == -1))
                return false;
        }
        return true;
    }
    public static void main(String[] args) {
        Main sol = new Main();
        String password1 = "cospro890";
        boolean ret1 = sol.solution(password1);
        System.out.println("solution 메소드의 반환 값은 " + ret1 + " 입니다.");

        String password2 = "cba323";
        boolean ret2 = sol.solution(password2);
        System.out.println("solution 메소드의 반환 값은 " + ret2+ " 입니다.");       
    }
}
```

### 2차) 문제10  - 
```Java

```


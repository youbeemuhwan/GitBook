# 2016년

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

```
class Solution {
    public String solution(int a, int b) {
      String [] day = {"FRI","SAT","SUN","MON","TUE","WED","THU"};
        int [] date = {31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        int select = 0;

        for(int i = 0; i < a-1; i++ ){
            select += date[i];
        }

        select += b-1;
        return day[select % 7];
    
    }
}
```

## 풀이

1월 부터 12월 까지의 일수를 배열에 담아서 a -1 까지의 일수를 select 에 더하고 나머지 a의 일수를 더해준뒤 7로 나눈 값을 day 의 인덱스에 넣어 해당 요일을 꺼내왔다.






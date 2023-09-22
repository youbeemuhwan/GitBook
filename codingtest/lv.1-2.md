# \[Lv.1] 예산

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

```
import java.util.*;

class Solution {
    public int solution(int[] d, int budget) {
        int result = 0;
        Arrays.sort(d);
       for(int i = 0; i < d.length; i++){
          if(budget < d[i]){
              break;
          }
           
           budget -= d[i];
           result ++;
       } 
        return result;
}
    }
```

## 풀이

그래서 최대한 많은 부서의 물품을 구매해 줄 수 있도록 하려고 합니다. 게속 반은 성공 반은 실패여서 어떤 부분인가 했는데 해당 문구를 놓쳐 Arrays.sort 로 내림차순을 안해준 상태에서 테스트를 돌렸다.

문제 풀때 맘을 급하게 먹게 되는데 고쳐야 하는 부분이다.

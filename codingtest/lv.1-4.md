# \[Lv.1] 삼총사

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## 풀이



```
import java.util.*;

class Solution {
    public int solution(int[] number) {
        int idx = 0;

        for (int i = 0; i < number.length; i++) {
            for (int j = i+1; j < number.length; j++) {
                for (int k =j+1; k < number.length; k++){
                    if(number[i] + number[j] + number[k] == 0){
                        idx ++;
                    }
                }

            }
        }
        return idx;
    }
}
```

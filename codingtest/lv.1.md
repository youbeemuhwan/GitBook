# \[Lv.1] : 추억 점수

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## 풀이

&#x20;(name :  yearning)  Map 형태로 잡고 photo 를 이중 배열을 돌려서 Map 의 키값 과 해당 photo \[] 의 요소가 일치하면 Map의 value 값을 score 라는 변수에 추가 해주고 int\[] result 에 순서에 맞게 저장한다.



일일히 photo 요소랑 map 키 값을 맞출려고 했는데 그 방법보다 **map.ContainsKey 메서드**를 활용하니 더 효율적이였다.&#x20;



## 정답

<pre><code>import java.util.*;

class Solution {
    public int[] solution(String[] name, int[] yearning, String[][] photo) {
      
        Map&#x3C;String, Integer> nameMap = new HashMap&#x3C;>();

        int [] result;
        result = new int[photo.length];

        for (int i = 0; i &#x3C;= name.length-1; i++){
            nameMap.put(name[i], yearning[i]);
        }

        for(int i = 0; i &#x3C;= photo.length-1; i++){
            int score =0;
            for(int j = 0; j  &#x3C;=photo[i].length-1; j++){
                if(nameMap.containsKey(photo[i][j])){
                    score += nameMap.get(photo[i][j]);
                }
            }
            result[i] = score;
        }
        return result; 
<strong>    }
</strong>}
</code></pre>




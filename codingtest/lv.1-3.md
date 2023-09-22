# \[Lv.1] 이상한 문자 만들기

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>



## **첫 번째 풀이**

```
 public String solution(String s) throws Exception {

        String result = "";

        String[] splitedStrings = s.split("\\s");


        for (int k =0; k< splitedStrings.length; k++){
            System.out.println("splitedStrings = " + splitedStrings[k]);
        }

        System.out.println("splitedStrings = " + splitedStrings);

        for(int i = 0; i < splitedStrings.length; i++){
            for(int j = 0; j < splitedStrings[i].length(); j++){
                if(j % 2 == 0 || j == 0){
                    String string = String.valueOf(splitedStrings[i].charAt(j));
                    String upperCase = string.toUpperCase();
                    result += upperCase;
                }
                else if(j % 2 != 0){
                    String string = String.valueOf(splitedStrings[i].charAt(j));
                    String lowerCase = string.toLowerCase();
                    result += lowerCase;
                }

            }
            result += " ";

        }
        return result;

```

split.() 을 공백 별로 나누어 단어 당 하나의 인덱스에 담았지만 이럴경우 공백이 여러개 일 경우에 대한 반례에 대처하지 못했다.&#x20;

## 수정 후 풀이

```
class Solution {
    public String solution(String s) {
     
     String result = "";

        int idx = 0;

        String[] splitedStrings = s.split("");

        for (int i = 0; i < splitedStrings.length; i++) {

            if (splitedStrings[i].equals(" ")) {
                idx = 0;
                
            } else if (idx % 2 == 0 || idx == 0) {
                splitedStrings[i] = splitedStrings[i].toUpperCase();
                idx++;

            } else if (idx % 2 != 0) {
                splitedStrings[i] = splitedStrings[i].toLowerCase();
                idx++;

            }
            result += splitedStrings[i];
        }

        return result;


    }
}
```

단어에 글자 하나씩 나눈 뒤 연산을 했다.

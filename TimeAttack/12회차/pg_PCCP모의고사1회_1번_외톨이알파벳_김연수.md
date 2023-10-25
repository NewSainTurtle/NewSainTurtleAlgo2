### PG121683 외톨이 알파벳

[📁문제보기](https://school.programmers.co.kr/learn/courses/15008/lessons/121683)

---

#### 문제 풀이

* 알파벳 개수를 세어 배열에 저장
* 문자열 맨 앞에서부터 한 글자씩 탐색하면서 count를 세는데 연속적인 같은 알파벳은 하나로 처리함
* count값이 2개이상인 알파벳만 Stringbuilder에 담아 출력
---

#### 전체 코드

```java
import java.io.*;
import java.util.*;

class Solution {
    public String solution(String input_string) {
        int[] cnt = new int['z'-'a'+1];
        char[] str = input_string.toCharArray();
        
        char temp = str[0];
        cnt[str[0]-'a']++;
        for(int i = 1;i<str.length;i++){
            if(temp==str[i]) continue;
            else {
                cnt[str[i]-'a']++;
                temp = str[i];
            }
        }
        
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<cnt.length;i++){
            if(cnt[i]>=2){
                sb.append((char)(i+'a'));
            }
        }        
        if(sb.length()==0) sb.append('N');
        
        return sb.toString();
    }
}
```

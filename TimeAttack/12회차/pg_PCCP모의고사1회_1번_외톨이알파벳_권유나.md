## [👾](https://school.programmers.co.kr/learn/courses/15008/lessons/121683) [#1번] 외톨이 알파벳 

> **소요 시간: 9분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

* 연속되지 않은 알파벳이 2개 이상 있는지 체크
### 전체 코드

```java
class Solution {
    public String solution(String input_string) {
        int[] count_alphabets = new int[26];
        int N = input_string.length();
        int idx = -1;
        while(++idx < N){
            char target = input_string.charAt(idx);
            if(count_alphabets[target-'a'] == 2){
                continue;
            }
            
            count_alphabets[target-'a']++;
            
            while(idx+1 < N && target==input_string.charAt(idx+1)){
                idx++;
            }
        }
        
        StringBuilder sb = new StringBuilder();
        for(int i=0; i<26; i++){
            if(count_alphabets[i]==2){
                sb.append((char)(i+'a'));
            }
        }
        return sb.length()==0? "N":sb.toString();
    }
}
```

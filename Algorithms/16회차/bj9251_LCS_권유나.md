## [🍅](https://www.acmicpc.net/problem/9251) [bj9251] LCS

> **소요 시간: 5분<br>
> 메모리: 15748KB<br>
> 시간: 104ms**
## 문제 접근
- LCS(Longest Common Subsequence)를 공부하기 위해서 문제 선정!
## 문제 풀이

1. 각 문자열 앞에 빈 문자를 넣는다고 생각하고, `lcs_arr`를 각 문자열+1 크기로 이차원 배열을 만든다.
2. `B`문자열의 문자와 `A`문자열의 문자를 비교한다.
	1. 두 문자가 같다면, 이전 LCS의 길이에 1을 더한 값을 저장한다. (= `lcs_arr[b][a]`에 `lcs_arr[b-1][a-1]+1`을 저장한다.)
	2. 두 문자가 다르다면, 비교한 문자가 포함된 LCS값을 저장한다. (= `lcs_arr[b][a]`에 `lcs_arr[b-1][a]`와 `lcs_arr[b][a-1]`중 더 큰 값을 저장한다.)
3. 길이를 출력하면 되므로 `lcs_arr[B.length][A.length]` 값을 출력한다.
## 전체 코드

```java
import java.io.BufferedReader;  
import java.io.InputStreamReader;  
  
public class Main_9251 {  
  
    public static void main(String[] args) throws Exception {  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        char[] A = br.readLine().toCharArray();  
        char[] B = br.readLine().toCharArray();  
        int[][] lcs_arr = new int[B.length + 1][A.length + 1];  
  
        for (int b = 1; b <= B.length; b++) {  
            for (int a = 1; a <= A.length; a++) {  
                if (A[a - 1] == B[b - 1]) {  
                    lcs_arr[b][a] = lcs_arr[b - 1][a - 1] + 1;  
                } else {  
                    lcs_arr[b][a] = Math.max(lcs_arr[b - 1][a], lcs_arr[b][a - 1]);  
                }  
            }  
        }  
        System.out.println(lcs_arr[B.length][A.length]);  
    }  
}
```

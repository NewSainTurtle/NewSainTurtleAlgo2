## [📚](https://school.programmers.co.kr/learn/courses/15009/lessons/121688) [2번] 신입사원 교육 

> **소요 시간: 16분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

* 주어진 입력 범위 신입사원 수의 최대가 1,000,000이고, 능력치 최대가 100이었다.
* 1~100 사이로 주어지기 때문에 능력치가 중복될 수 있다고 생각했고, 백만을 정렬하는 것보다 백을 정렬하는게 더 시간이 단축되기 때문에 `Map`을 사용하였다.
* `Map` 중에서도 자동정렬이 되는 `TreeMap`을 사용
* 중복 코드를 줄이기 위해서 메소드를 작성해서 재사용함
### 전체 코드

```java
import java.util.TreeMap;
import java.util.Map;

class Solution {
    TreeMap<Integer, Integer> map;
    public int solution(int[] ability, int number) {
        map = new TreeMap();
        for(int a: ability){
            putAbility(a,1);
        }
        
        while(number-- > 0){
            int A = getMinAbility();
            int B = getMinAbility();
            putAbility(A+B,2);
        }
        
        int answer = 0;
        for(Map.Entry<Integer, Integer> entry : map.entrySet()){
            answer += (entry.getKey()*entry.getValue());
        }
        return answer;
    }
    
    int getMinAbility(){
        int a = map.firstKey();
        if(map.get(a)==1){
            map.remove(a);
        }else{
            map.replace(a,map.get(a)-1);
        }
        return a;
    }
    
    void putAbility(int a, int cnt){
        if(map.containsKey(a)){
            map.replace(a,map.get(a)+cnt);
        }else{
            map.put(a,cnt);
        }
    }
}
```

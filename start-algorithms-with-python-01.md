## 알고리즘 준비운동
* 목적 : 알고리즘 문제풀이에 필요한 파이썬의 접근방식을 익혀보자!


#### 1. 알고리즘을 작성에 대한 고려사항
* 주어진 문제를 프로그래밍 언어로 작성해서 원하는 결과를 출력하게 만들면 모두 좋은 알고리즘일까?
* 당연히 아니다! 컴퓨터는 한정(CPU,RAM,..)된 자원을 바탕으로 작업(연산)을 진행한다. 그러므로 알고리즘을 작성할때는 효율적인 알고리즘이 무엇인지 항상 생각해야한다.
* 그렇다면 효율적인 알고리즘을 판단하는 요소는 무엇일까? 그것은 바로 시간복잡도(time complexity)와 공간복잡도(space complexity)이다!
https://en.wikipedia.org/wiki/Time_complexity
https://en.wikipedia.org/wiki/DSPACE

* 발전된 하드웨어 스펙으로인해 최근에는 공간복잡도에 대한 최적화가 시간복잡도보다는 덜 이루어진다. 
* 여기서는 시간복잡도에 대한 접근 방식을 살펴보겠다.


#### 2. 시간복잡도
* 
![Alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7e/Comparison_computational_complexity.svg/512px-Comparison_computational_complexity.svg.png)





#### 3. 실전 문제!
* 그럼 간단한 문제를 풀어보자. 다음과 같이 중복된 값이 오직 한개만 존재하는 배열이 존재 할 때 중복된 요소는 어떻게 찾을 수 있을까? 
> [6, 23, 5, 12, 46, 71, 9, 23]

* 먼저 시간복잡도를 고려하지 않고 가장 쉽게 생각하면 다음과 같지 않을까?
> 먼저 배열의 첫 번째 요소를 나머지 요소 전부와 비교해서 똑같은 값이 있는지 검사한다. 다음은 두 번째 요소와 나머지 요소 전부와 비교해서 똑같은 값이 있는지 검사한다. ... 마지막 요소를 나머지 요소와 비교해서 똑같은 값이 있는지 검사한다.
* 위와 같은 알고리즘을 파이썬으로 표현하면 다음과 같다.
```python
def checkDuplcation(arr):
    length = len(arr)
    for i in range(length):
        for j in range(length):			
            if i == j:
	        continue
	    elif arr[i] == arr[j]:				
	        return arr[i]
    return -1


def main():
    arr = [6, 23, 5, 12, 46, 71, 9, 23]
    print(checkDuplcation(arr))
	
if __name__ == "__main__":
    main()
```
* 위의 알고리즘을 설명하면 다음과 같다.
1. 반복문 두개가 중첩되어 존재한다. 
2. 같은 인덱스의 요소를 비교하는 것을 조건분으로 피한다.
3. 중복된 요소가 존재하면 중복된 값을 반환하고 존재하지 않으면 -1을 반환한다.
* 위의 알고리즘은 어떠한 배열(문제에서 제시한 특성을 가지는)이 와도 중복된 요소를 찾을 수 있다.
* 그렇다면 성능적인 면에서 봤을 때 효율적일까? 조금만 생각해보면 다음과 같이 중복된 연산이 일어나는 것을 알 수있다.
> 바깥 i가 0일때 arr[0]과 arr[1]을 비교했는데 i가 1일때 arr[1]과 arr[0]의 비교가 또 이루어진다. 즉 중복된 연산이 이루어진다. 
* 
 


#### 3. 개선1 - 중복연산제거
* 여기서는 앞에서 설명한 중복된 연산을 제거해보자.
* 어떻게 중복된 연산을 제거할 수 있을까... 계속 생각해보자... 
> 첫 번째 요소와 나머지 요소를 연산한 후 다음 요소부터는 첫번째 요소와 검사 할 필요가 없다. 즉, 두번째 요소를 검사할때는 첫번째가 아닌 두번째 부터 중복여부를 검사하면된다. 하지만 두번째 요소또한 같은 요소를 비교하는 것이기 때문에 검사할 필요가 없다.
<pre>
[6, 23, 5, 12, 46, 71, 9, 23]
1. 첫 번째 값을 두번째 값부터 비교하며 검사
2. 두 번째 값을 세번째 값부터 비교하며 검사
</pre>
* 코드는 다음과 같다. 사실 코드레벨의 차이점이라고는 for j in range(i+1, length): 부분 밖에 없다.
```python
def checkDuplcation(arr):
    length = len(arr)
    for i in range(length):
        for j in range(i+1, length):			
            if i == j:
	        continue
	    elif arr[i] == arr[j]:				
	        return arr[i]
    return -1


def main():
    arr = [6, 23, 5, 12, 46, 71, 9, 23]
    print(checkDuplcation(arr))

if __name__ == "__main__":
    main()
```


#### 4. 개선2 - 자료구조 이용하기
* 좀더 개선하기 위해서는 어떤 자료구조를 사용하면 좋을까?
* 이전의 개선과 같이 계속 생각해보자... 계속 생각해보자...
> 
* 답을 먼저 말하면 해시(hash)를 이용하는 것이다.
* 
* **O(N)**
* 코드는 다음과 같다.
```python
def checkDuplcation(arr):
    dic = {}
    length = len(arr)
    for i in range(length):
        if arr[i] in dic:
	    return arr[i]
	else:
	    dic[arr[i]] = 1
    return -1


def main():
    arr = [6, 23, 5, 12, 46, 71, 9, 23]
    print(checkDuplcation(arr))

if __name__ == "__main__":
    main()
```
* 여기서 중요한 사실은 **파이썬의 딕셔너리는 해쉬로 작동**한다는 점이다.
* 
* 




#### 5. 마무리
* 최신 기술의 프레임워크, 라이브러리등에는 많은 관심을 가지지만, 정작 프로그램의 본질적인 완성도를 높이는 자료구조, 알고리즘을 등한시 하는 개발자들이 있다.
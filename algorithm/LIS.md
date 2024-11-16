# Longest Increasing Subsequence, 최장 증가 부분 수열

> LIS 란, 원소가 N 개인 배열의 일부 원소를 골라내서 만든 부분 수열 중, 각 원소가 이전 원소보다 크다는 조건을 만족하고, 그 길이가 최대인 부분 수열을 ` 최장 증가 부분 수열` 이라고 합니다. 

## DP 를 활용한 풀이
일반적으로 최장 증가 부분 수열의 길이가 얼마인지 푸는 간편한 방법은 *`DP`* 를 이용하는 것입니다. 하지만 시간 복잡도는 `O(N^2)` 로 인풋 값이 백만 개 정도만 되어도 알고리즘 실행시간이 10 초 이상 소요됩니다.<br>
[가장 긴 증가하는 부분 수열](https://www.acmicpc.net/problem/11053 "11053") 문제를 예시로 들어보겠습니다.

수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {**10**, **20**, 10, **30**, 20, **50**} 이고, 길이는 **4** 입니다.

```java
int[] len = new int[N];

for (int i = 0; i < N; i++) {
    len[i] = 1;
    for (int j = 0; j < i; j++) {
        if (arr[i] > arr[j])
            len[i] = Math.max(len[i], len[j] + 1);
    }
}
```

여기서 `len[i]` 는 i번째 인덱스에서 끝나는 최장 증가 부분 수열의 길이를 의미합니다. <br/>
우선적으로 len[i] = 1로 초기화합니다. 이는 자기 자신만 포함되는 것을 의미합니다. 주어진 배열에서 인덱스를 한 칸씩
늘려가면서 확인합니다. 그리고 내부 반복문으로 i보다 작은 인덱스들을 하나씩 살펴보면서 arr[i] > arr[j] 인 것이 있을 경우,
len[i] 를 업데이트합니다.


그 결과 최장 증가 부분 수열의 길이의 배열이 `len ` 에 저장되고, 이를 `stream` 을 통해 최대값을 출력하면 됩니다.

```java
import java.util.stream.*;

..
..

int MAX = Arrays.stream(len)
        .max()
        .getAsInt();

System.out.println(MAX);
```

## 이분 탐색을 이용한 방법
이분 탐색, Binary Search  를 사용하면 시간 복잡도를 `O (NlogN)` 으로 줄일 수 있습니다.

```java
int binary_search(vector<int> lis, int start, int end, int element) {
    //이분 탐색으로 lis 벡터 내에서 element의 위치를 반환
    //lis 벡터의 start - end 구간에서만 확인
    while (start < end) {
        int mid = (start + end) / 2;
        if (element > lis[mid]) start = mid + 1;
        else end = mid;
    }
    return end;
}
 
int LIS_BS() {
    int ret = 0;
    vector<int> lis;
    lis.push_back(arr[0]);
 
    for (int i = 1; i < n; i++) {
        //만약 lis 벡터의 마지막 수보다 i번째 수가 크다면, 그냥 뒤에 붙인다.
        if (arr[i] > lis.back()) {
            lis.push_back(arr[i]);
            ret = lis.size() - 1;
        }
        //i번째 수에 대해, lis 벡터 내에서 그 수의 위치를 찾는다.
        int pos = binary_search(lis, 0, ret, arr[i]);
        lis[pos] = arr[i];
    }
 
    return ret + 1;
}
```
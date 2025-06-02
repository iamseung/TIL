## [1] 버블 정렬 
- 인접한 두 값을 비교하여 큰 값을 뒤로 버블처럼 밀어내는 방식
- 시간 복잡도 : N^2
- 코드가 단순하지만 매우 느림.

```java
public static void bubbleSort(int[] arr) {
    int n = arr.length;

    for(int i = 0; i < n - 1; i++) {
        for(int j = 0; j < n - 1; j++) {
            if(arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

## [2] 선택 정렬
- 가장 작은 값을 선택해서 맨 앞과 교체
- 시간 복잡도 : N^2
- 메모리 사용이 적지만 느리고 안정 정렬이 아니다.

```java
public static void selectionSort(int[] arr) {
    int n = arr.length;

    for(int i = 0; i < n - 1; i++) {
        // 좌측 커서
        int minIdx = i;

        // 좌측 커서 이후부터 비교
        for(int j = i + 1; j < n; j++) {
            if(arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }

        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}
```

## [3] 삽입 정렬
- 앞에서부터 하나씩 정렬된 위치에 삽입
- 시간 복잡도 : N^2, 거의 정렬된 경우 O(n)
- 구현 간단, 안전 정렬, TimSort 내부 일부로 쓰임
    
```java
public static void insertionSort(int[] arr) {
    int n = arr.length;

    for(int i = 1; i < n ; i++) {
        int key = arr[i];

        int k = i - 1;

        // 현재 위치보다 왼쪽을 비교하며 자신보다 큰 값은 뒤로 밀고 자리 자리를 찾아서 삽입
        while(k >= 0 && arr[k] > key) {
            arr[k + 1] = arr[k];
            k--;
        }

        arr[k + 1] = key;
    }
}
```
 
## [4] 퀵 정렬
- 피벗 기준으로 좌우로 나누어 정렬 (분할 정복)
- 시간 복잡도 : 평균 O(n log n), 최악은 N^2
- 매우 빠르고 실무에서도 자주 사용, 불안정 정렬 & 구현 복잡

```java
public static void main(String[] args) {
    int[] arr = {3,5,1,74,23,57,2,99,13,47,12};
    quickSort(arr, 0, arr.length - 1);
    System.out.println("QuickSort: " + Arrays.toString(arr));
}

public static void quickSort(int[] arr, int left, int right) {
    if (left >= right) return;

    int pivot = arr[(left + right) / 2];
    int i = left, j = right;

    while (i <= j) {
        while (arr[i] < pivot) i++;
        while (arr[j] > pivot) j--;

        if (i <= j) {
            int tmp = arr[i];
            arr[i++] = arr[j];
            arr[j--] = tmp;
        }
    }

    quickSort(arr, left, j);
    quickSort(arr, i, right);
}
```
 
## [5] 병합 정렬
- 배열을 계속 반으로 나눈 뒤 정렬하여 합침
- 시간 복잡도 평균 O(n log n)
- 안전 정렬, 공간 많이 사용 (배열 복사 필요)

```java
public static void mergeSort(int[] arr) {
    if (arr.length < 2) return;

    int mid = arr.length / 2;
    int[] left = Arrays.copyOfRange(arr, 0, mid);
    int[] right = Arrays.copyOfRange(arr, mid, arr.length);

    mergeSort(left);
    mergeSort(right);
    merge(arr, left, right);
}

private static void merge(int[] arr, int[] left, int[] right) {
    int i = 0, j = 0, k = 0;

    while (i < left.length && j < right.length) {
        arr[k++] = (left[i] <= right[j]) ? left[i++] : right[j++];
    }

    while (i < left.length) arr[k++] = left[i++];
    while (j < right.length) arr[k++] = right[j++];
}
```

## [6] 퀵 정렬
- 피벗 기준으로 좌우로 나누어 정렬 (분할 정복)
- 시간 복잡도 : 평균 O(n log n)
- 메모리 적게 사용, 불안정 정렬, MergeSort 보다 느릴 수 있음

```java
public static void heapSort(int[] arr) {
    int n = arr.length;

    // Build max heap
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // Extract elements
    for (int i = n - 1; i > 0; i--) {
        int tmp = arr[0];
        arr[0] = arr[i];
        arr[i] = tmp;

        heapify(arr, i, 0);
    }
}

private static void heapify(int[] arr, int size, int i) {
    int largest = i;
    int l = 2 * i + 1;
    int r = 2 * i + 2;

    if (l < size && arr[l] > arr[largest]) largest = l;
    if (r < size && arr[r] > arr[largest]) largest = r;

    if (largest != i) {
        int tmp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = tmp;
        heapify(arr, size, largest);
    }
}
```
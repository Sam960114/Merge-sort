發牢騷與問題點
===
老實說在學習排序法的時候，這個合併排序法真的把我搞到頭昏眼花，完全看不懂程式碼是如何運作的，令人難以理解。 

而我也真的就是傻傻的跟他死嗑到底，就只是為了搞懂這段程式碼在幹什麼。
```
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        // 找到中點
        int mid = left + (right - left) / 2;

        // 遞迴對左半部分進行排序
        mergeSort(arr, left, mid);
        
        // 遞迴對右半部分進行排序
        mergeSort(arr, mid + 1, right);

        // 合併兩個已排序的部分
        merge(arr, left, mid, right);
    }
}
```





合併排序法最重要的概念就是把一段不規則連續的數字不斷拆解，並在分解到最小項目的時候按照大小順序重組。如下圖：
![Merge_sort_algorithm_diagram.svg](https://hackmd.io/_uploads/SkWgxRd0A.png)


而我當時搞不懂的地方就是在★的部分：
```
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        // 找到中點
        int mid = left + (right - left) / 2;

        // 遞迴對左半部分進行排序
        ★1 mergeSort(arr, left, mid);
        
        // 遞迴對右半部分進行排序
        ★2 mergeSort(arr, mid + 1, right);
        
        ...
```
我當時不理解假如程式碼運行到★1的時候開始遞迴的話，★2程式要如何運行?


直到某天在睡覺的時候睡到一半，腦袋突然靈光乍現，這才成功參透"合併排序法"的運作。

真是南無阿密豆腐(。・ω・。)



---
正文
===
廢話這麼多，現在就開始進入正文ㄅ

要解答這個疑惑我們需要先幫程式碼列個編號，編號大致如下:

```
void mergeSort(...) {
    if (left < right) {
        
        ...

        分割左端
        Ａ：mergeSort(arr, left, mid);
        
       分割右端
        Ｂ：mergeSort(arr, mid + 1, right);

        // 合併兩個已排序的部分
        Ｃ：merge(arr, left, mid, right);
```

我當時想說，當程式跑到Ａ的時候，又會遞迴重跑一次，接著到Ａ的時候又再遞迴一次
```
If=>Ａ=>B=>C
    ↓
    If=>A
        ↓
        If=>A
            ↓
            If=>A
                無限循環
```

以上是我當時的理解，我當時只專注於A會跑到哪裡，完全沒有注意到後續發展。

而接著就是我頓悟後的理解:

```
If=>Ａ=>B=>C
    ↓
    If=>A=>B=>C
        ↓
        If=>A=>B=>C
            ↓  ⇧
            If(當程式不滿足時)
```

沒錯!程式開始回推了!發現的當下我真的很驚訝，那些發明合併排序法的程式設計師究竟想到了第幾層，才能創造出這段程式



---
總結
===
為了合併排序法，我花了約一周的時間去理解，過程中除ㄌ



---
完整程式碼
===

```
#include <iostream>
#include <vector>

using namespace std;

// 合併兩個子陣列：arr[left..mid] 和 arr[mid+1..right]
void merge(vector<int>& arr, int left, int mid, int right) {
    // 計算兩個子陣列的大小
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // 建立臨時陣列 L 和 R
    vector<int> L(n1), R(n2);

    // 複製資料到臨時陣列 L 和 R
    for (int i = 0; i < n1; i++) 
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++) 
        R[j] = arr[mid + 1 + j];

    // 初始化子陣列索引和主陣列索引
    int i = 0, j = 0, k = left;

    // 合併過程：比較 L 和 R 中的元素，並放入 arr 中
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // 複製剩餘的 L 元素（如果有）
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // 複製剩餘的 R 元素（如果有）
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

// 遞迴地拆分並排序陣列
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        // 找到中點
        int mid = left + (right - left) / 2;

        // 遞迴對左半部分進行排序
        mergeSort(arr, left, mid);
        
        // 遞迴對右半部分進行排序
        mergeSort(arr, mid + 1, right);

        // 合併兩個已排序的部分
        merge(arr, left, mid, right);
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    int arr_size = arr.size();

    cout << "原始陣列: ";
    for (int i = 0; i < arr_size; i++)
        cout << arr[i] << " ";
    cout << endl;

    mergeSort(arr, 0, arr_size - 1);

    cout << "排序後陣列: ";
    for (int i = 0; i < arr_size; i++)
        cout << arr[i] << " ";
    cout << endl;

    return 0;
}

```
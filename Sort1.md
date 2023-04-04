# Coding-Meetup

## Sort-排序
### 何謂排序?
指將輸入的數由小到大或由大到小重新排列。

### 為何有這麼多不同種類的排序?
為了因應不同場景與條件，可挑選最適用的排序法。<br/>

<img width="857" alt="image" src="https://user-images.githubusercontent.com/89962742/229693537-327ca450-36b4-4321-87b1-6b597f98645c.png">
穩定v.s.不穩定：相同的元素是不是有可能順序改變。

### 常用的排序-Bubble Sort、Selection Sort、Insertion Sort、Quick Sort、(Merge Sort、Bucket Sort、Heap Sort、Cocktail Sort...etc)
### Bubble Sort 
重複遍歷要排序的數列，比較相鄰兩個元素的大小，如果前一個元素比後一個元素大，則交換兩個元素的位置。

![bubble1](https://user-images.githubusercontent.com/89962742/229695426-4c2b3d65-7993-4392-99b3-fd597595d5d8.jpg)
![bubble2](https://user-images.githubusercontent.com/89962742/229695453-7c9c8d89-3d78-45a7-9725-052d396e2bb1.jpg)
![Bubble_sort_animation](https://user-images.githubusercontent.com/89962742/229695365-8b3d07fe-eda1-4b70-809c-86a34cd22eb6.gif)

```JAVA
public class BubbleSort {
    
    public static void main(String[] args) {
        int[] arr = { 5, 3, 8, 4, 2 };
        bubbleSort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        int temp = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < (n - i); j++) {
                if (arr[j - 1] > arr[j]) {
                    temp = arr[j - 1];
                    arr[j - 1] = arr[j];
                    arr[j] = temp;
                }
            }
        }
    }
}
```
優點：淺顯易懂，暴力輕鬆解。
缺點：O(n^2)，效率差。

### Selection Sort
重複地選擇剩餘元素中的最小值，然後將其放入已排序的序列中，直到所有元素均已排序完畢。
![Selection](https://user-images.githubusercontent.com/89962742/229697713-865de8e8-ef14-40c3-81ea-c3ff94d90c7c.jpg)
![Selection_sort_animation](https://user-images.githubusercontent.com/89962742/229697821-5c317920-5203-4b74-a15f-82c629bd08fa.gif)
```JAVA
public class SelectionSort {
    
    public static void main(String[] args) {
        int[] arr = { 5, 3, 8, 4, 2 };
        selectionSort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void selectionSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            if (minIndex != i) {
                int temp = arr[i];
                arr[i] = arr[minIndex];
                arr[minIndex] = temp;
            }
        }
    }
}
```
優點：淺顯易懂，由於每次只需要進行一次交換，故速度比泡沫稍快一點點。
缺點：O(n^2)，效率差，為不穩定排序。

### Insertion Sort
將未排序的元素插入到已排序的序列中，來完成排序。
![Insertion1](https://user-images.githubusercontent.com/89962742/229698362-4cb3d1a6-3191-468d-8dea-ac99be79297b.jpg)
![Insertion2](https://user-images.githubusercontent.com/89962742/229698397-f40a833c-aa48-4457-8eaa-6c4435a816fe.jpg)
![Insertion_sort_animation](https://user-images.githubusercontent.com/89962742/229698445-f1631fc2-f27e-4e5a-90c3-9fd2f57af724.gif)

```JAVA
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }

    public static void main(String[] args) {
        int[] arr = { 12, 11, 13, 5, 6 };
        insertionSort(arr);
        System.out.println("Sorted array:");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

優點：淺顯易懂，是穩定的排序法。
缺點：O(n^2)，效率差。插入vs選擇 => 看需不需要穩定排序 
若需要 => 插入排序
不需要 => 若資料為部分有序 => 插入 > 選擇
          若資料為無序 => 選擇 > 插入

### Quick Sort
選擇一個基準值，然後將數列化分為兩部分，左邊部分的元素均小於基準值；右邊部分的元素均大於基準值，將數列分成子問題然後對這兩個部分進行遞迴排序。
![quick1](https://user-images.githubusercontent.com/89962742/229699486-734473cd-c20b-4523-bee6-a39cc18f3a2e.jpg)
![quick2](https://user-images.githubusercontent.com/89962742/229699531-b6539a63-de58-4375-8613-6b0628fa90df.jpg)
![quick3](https://user-images.githubusercontent.com/89962742/229699554-bc67d23a-21a3-4b79-9708-1300ce9e32d0.jpg)
![Sorting_quicksort_anim](https://user-images.githubusercontent.com/89962742/229699598-4d27ac74-01bb-44c9-a4f0-530899b7a5c7.gif)
```JAVA
public class QuickSort {
	public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivot = partition(arr, low, high);
            quickSort(arr, low, pivot - 1);
            quickSort(arr, pivot + 1, high);
        }
    }

    public static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        return i + 1;
    }

    public static void main(String[] args) {
        int[] arr = { 64, 25, 12, 22, 11 };
        int n = arr.length;
        quickSort(arr, 0, n - 1);
        System.out.println("Sorted array:");
        for (int i = 0; i < n; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

## Sort Practice

### Leetcode 75.Sort Colors
給定一個數字陣列1,2,3分別代表紅、白、藍三種顏色，用升冪重新排列，不能用lib的Arrays.Sort method
Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

Example 1:
```JAVA
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
Example 2:
```JAVA
Input: nums = [2,0,1]
Output: [0,1,2]
```

Constraints：
* n == nums.length
* 1 <= n <= 300
* nums[i] is either 0, 1, or 2.

Bubble Sort Sol.<br/>
<img width="607" alt="image" src="https://user-images.githubusercontent.com/89962742/229701372-91ed6297-6a62-4f91-8f63-4ed3534a5dba.png"><br/>
Selection Sort Sol.<br/>
<img width="593" alt="image" src="https://user-images.githubusercontent.com/89962742/229701696-a6a1ab13-2e14-4932-9f14-b9f3531c0ce8.png"><br/>
Insertion Sort Sol.<br/>
<img width="617" alt="image" src="https://user-images.githubusercontent.com/89962742/229701900-33901596-5659-4a9a-87d1-99389773c664.png"><br/>

### Leetcode 215.Kth Largest Element in and Array
給定一個陣列與數字k，回傳排序後第K大的數字，不用distinct
Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

You must solve it in O(n) time complexity.

Example 1:
```JAVA
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```
Example 2:
```JAVA
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```
Constraints:

* 1 <= k <= nums.length <= 105
* -104 <= nums[i] <= 104

Quick Sort Sol.<br/>
<img width="600" alt="image" src="https://user-images.githubusercontent.com/89962742/229704467-abd51348-3cb9-4dc0-bbf6-a2432c5c069d.png">

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
		int[] array = {5,3,8,4,2};
		int[] result = bubbleSort(array);
		for(int num : result) {
			System.out.print(num);
		}
	}
	
	private static int[] bubbleSort(int[] array) {
	    int temp;
	    for (int i = 0; i < array.length - 1; i++) {
	        boolean Flag = false; // 是否發生交換。没有交換，提前跳出外層循環
	        for (int j = 0; j < array.length - 1 - i; j++) {
	            if (array[j] > array[j + 1]) {
	                temp = array[j];
	                array[j] = array[j + 1];
	                array[j + 1] = temp;
	                Flag = true;
	            }
	        }
	        if (!Flag)
	        {
	            break;
	        }
	    }
	    return array;
	}

}
```
initial array：{5,3,8,4,2}<br/>
when i = 0(第一次循環), j 從 0 開始，array[j] 與 array[j+1] 進行比較，若array[j]比較大，則兩者互換位置。<br/>
第一次循環結束後，最大的數會在最右邊，如下：<br/>
{5,3,8,4,2} => {3,5,8,4,2} => 不動 => {3,5,4,8,2} => {3,5,4,2,8}<br/>
<img width="474" alt="image" src="https://user-images.githubusercontent.com/89962742/229796225-bd953d1f-f137-4daf-82db-f367aa426a01.png"><br/>
<img width="698" alt="image" src="https://user-images.githubusercontent.com/89962742/229796278-ff470049-75b5-4894-839f-9b4ee9fb1260.png"><br/>
到此時最大的8已經泡泡浮起來了，故第二次的j+1遍歷範圍不需要包含到8，因此j的最大範圍才會有-i的部分。<br/>
第二次循環將會是<br/>
{3,5,4,2,8} => 不動 => {3,4,5,2,8} => {3,4,2,5,8}<br/>
以此類推<br/>
i的遍歷範圍：由於每次只移動一個位置，想像若最小的數字在最右邊e.g.{5,2,4,3,1}，此時1要移動到最左邊需要4次，故元素最大所需的移動距離為length-1，需length-1次循環，故i的範圍為0~length-2

優點：淺顯易懂，暴力輕鬆解。<br/>
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
initial array : {5,3,8,4,2}
由於我們最終的結果會是升冪排序，因此最小的數最終將會在最左邊(index為0)的地方，首先定義minIndex為i(0)。<br/>
只要把最小、次小、次次小...的數找出來，最終最大的數將無須再次排列，故i的範圍只需在n-2內。<br/>
接著開始對該次循環進行遍歷，將i與i之後的每個元素進行比較，比較後取得最小的元素，與當前的i交換位置，如下：<br/>
i = 0時，minIndex為0，此時的array為{5,3,8,4,2}，將array[i] = 5 與 3,8,4,2逐個進行比較後得知最小的數為2，2與5交換位置，因此該次循環後，array變為 {2,3,8,4,5}<br/>
<img width="783" alt="image" src="https://user-images.githubusercontent.com/89962742/229802709-5ab20271-660f-4c95-a0d5-733c8721d541.png"><br/>
i = 1時，minIndex為1，此時的array為{2,3,8,4,5}，將array[i] = 3 與 8,4,5逐個進行比較後得知最小的數為3，無須交換位置，因此該次循環後，array依然為 {2,3,8,4,5}<br/>
<img width="858" alt="image" src="https://user-images.githubusercontent.com/89962742/229802845-371e6ed8-d0d5-4dbc-83c1-be577d02877a.png"><br/>
....以此類推<br/>

優點：淺顯易懂，由於每次只需要進行一次交換，故速度比泡沫稍快一點點。<br/>
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
        int[] arr = { 5,3,8,4,2 };
        insertionSort(arr);
        System.out.println("Sorted array:");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```
initial array : {5,3,8,4,2}<br/>
其遍歷法為，從index為1的地方開始遞增，與其左邊所有的元素進行比較，流程如下：<br/>
{5,3,8,4,2}取index為1的元素為3，與其左邊所有元素進行比較，因5>3，因此位置互換，此時第一次遍歷結果為{3,5,8,4,2}<br/>
<img width="920" alt="image" src="https://user-images.githubusercontent.com/89962742/229815564-d065865a-26da-4ea6-8b55-aa5d07e94502.png"><br/>
第二次遍歷開始，index為2的元素為8，其左邊所有的元素皆比8小，因此陣列不變，依然為{3,5,8,4,2}。<br/>
<img width="200" alt="image" src="https://user-images.githubusercontent.com/89962742/229815649-d2db610d-addc-406f-bf5a-ff0f184e036a.png"><br/>
第三次遍歷開始，index為3的元素為4，與其左邊的元素(8)比較後發現4比較小，因此8往右移動一格(此時陣列為{3,5,8,8,2})，再往左一格比較後發現4比5小，因此5往右移動一個(此時陣列為3,5,5,8,2)，再往左一格發現4比3大，因此將4往這個地方insert，最終此次遍歷後的陣列為{3,4,5,8,2}<br/>
<img width="775" alt="image" src="https://user-images.githubusercontent.com/89962742/229833091-4800343e-108b-4b2f-9ee5-d56513147350.png"><br/>
<img width="524" alt="image" src="https://user-images.githubusercontent.com/89962742/229833135-84623714-d1a8-4d4b-b513-401213547df3.png"><br/>
...依此類推


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
public class QuickSortExample {
	public static void quickSort(int[] arr, int lowIndex, int highIndex) {
		//step7：若arr只有一個元素，直接return
		if(lowIndex >= highIndex) {
			return;
		}
		
		//step1：Define pivot
		int pivot = arr[highIndex];
		
		//step2:Define left/right pointer
		int leftPointer = lowIndex;
		int rightPointer = highIndex;
		
		//step3:開始進行partition過程
		while(leftPointer < rightPointer) {	//partition中，左邊指標與右邊指標的範圍
			while(arr[leftPointer] <= pivot && leftPointer < rightPointer) {	//對於左邊指標指到的值，要求要小於等於pivot，若大於則會在後面swap方法與右指標的值更換位置
				leftPointer++;
			}
			while(arr[rightPointer] >= pivot && leftPointer < rightPointer) {	//對於右邊指標指到的值，要求要大於等於pivot，若小於則會在後面swap方法與左指標的值更換位置
				rightPointer--;
			}
			
			//step4：更換位置
			swap(arr,leftPointer,rightPointer);
		}
		
		//step5：脫離while迴圈，表示此時leftPointer與rightPointer相遇，更換pivot與當下leftPointer指向的元素
		swap(arr,leftPointer, highIndex);
		
		//step6：將Partition後的兩個子分區繼續進行遞迴的子問題處理
		quickSort(arr, lowIndex, leftPointer - 1);//比pivot小的左邊區域處理
		quickSort(arr, leftPointer + 1, highIndex);//比pivot大的右邊區域處理
		
	}
	
	//4.1定義swap方法
	private static void swap(int[] arr, int leftIndex, int rightIndex) {
		int temp = arr[leftIndex];
		arr[leftIndex] = arr[rightIndex];
		arr[rightIndex] = temp;
	}

	public static void main(String[] args) {
		int[] array = {3,1,8,9,4,5,7};
		int arrayLength = array.length;
		quickSort(array, 0, arrayLength-1);
		
		for(int i = 0; i < arrayLength; i++) {
			System.out.println(array[i] + ",");
		}
	}

}
```
initial array : {3,1,8,9,4,5,7}
先選擇一個pivot(基準值)，此處是選擇最右邊的元素7，排序後將會以這個基準值為界線分為兩個區塊，稱為partition的操作。將兩個區塊的partition內容作為子問題作遞迴處理，最終將得到排序後的結果。
初始狀態時，定義左右兩個pointer的位置。
<img width="367" alt="image" src="https://user-images.githubusercontent.com/89962742/229752717-464ef33f-db72-4428-b313-d2fa51efd390.png"><br/>
此時，若leftPointer指向的元素大於pivot，rightPointer指向的元素小於pivot，則兩者交換。<br/>
<img width="806" alt="image" src="https://user-images.githubusercontent.com/89962742/229753071-98cf3ce9-37e7-4e9d-803a-40017ae0056e.png"><br/>
交換後pointer繼續向前&交換元素。<br/>
<img width="711" alt="image" src="https://user-images.githubusercontent.com/89962742/229753841-cd464683-1043-4c92-86d1-58bd0a36602b.png"><br/>
此時，left&rightPointer相遇，將pivot與此index元素交換，則第一次partition完畢，pivot左右可分為兩個子問題。<br/>
<img width="735" alt="image" src="https://user-images.githubusercontent.com/89962742/229754833-3c1e5a8a-f0bc-4505-83cb-6ea0c81ee78d.png"><br/>
接下來先處理比pivot小的左邊區域問題，此時子問題的leftPointer為lowIndex、rightPointer為母問題的leftIndex-1。<br/>
<img width="807" alt="image" src="https://user-images.githubusercontent.com/89962742/229756743-840488b2-e442-4bf7-9215-c38402d7a9d7.png"><br/>
針對子問題進行相同的partition過程如下。<br/>
<img width="763" alt="image" src="https://user-images.githubusercontent.com/89962742/229757922-b23e610c-77b2-4f08-bac9-5d449934ec9e.png">
<img width="724" alt="image" src="https://user-images.githubusercontent.com/89962742/229758555-2aa01b2b-a900-4799-9457-abdab8c1b51f.png"><br/>
右邊區域問題也以相同邏輯進行partition。<br/>
<img width="601" alt="image" src="https://user-images.githubusercontent.com/89962742/229761011-e86151a4-2791-431c-858f-f3f7fe4104e2.png"><br/>
處理完之後，整個母問題與子問題的架構如下，便可整理出最後結果。<br/>
<img width="823" alt="image" src="https://user-images.githubusercontent.com/89962742/229759910-c73b2885-d045-428b-b1ae-92f7a93a3770.png">




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

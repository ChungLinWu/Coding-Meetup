# Coding-Meetup

### Merge Sort
平均時間複雜度：O(nlogn)
最壞/最佳時間複雜度:O(nlogn)
空間複雜度：O(n)
將原始資料分割成兩個子資料列，再持續將子資料列持續兩兩分割直到無法分割。再分別將這些無法分割的單個資料兩兩比較後排序合併，最後回復成原始資料的排序後結果。<br/>
![OIP (1)](https://user-images.githubusercontent.com/89962742/233765969-49eb0790-4da4-4b59-ae1e-6610d8336d62.jpg)<br/>
![Merge_sort_animation2](https://user-images.githubusercontent.com/89962742/233766831-bfcb3303-4ad0-49af-b354-eed4f3b9a4c8.gif)
```JAVA
public class MergeSort {
	public static void main(String[] args) {
		//initailize unsorted array
		Random random = new Random();
		int[] numbers = new int[10];
		
		for(int i = 0; i < numbers.length; i++) {
			numbers[i] = random.nextInt(10);
		}
		
		//print unsorted array
		System.out.println("Before:");
		printArray(numbers);
		
		//start mergeSort
		mergeSort(numbers);
		
		//print sorted array
		System.out.println("After:");
		printArray(numbers);
	}
	
	private static void printArray(int[] array) {
		for(int num : array) {
			System.out.println(num);
		}
	}
	
	private static void mergeSort(int[] inputArray) {
		int inputArrayLength = inputArray.length;
		//return if only have one element
		if(inputArrayLength < 2) {
			return;
		}
		//seperate inputArray into two array
		int midIndex = inputArrayLength/2;
		int[] leftArray = new int[midIndex];
		int[] rightArray = new int[inputArrayLength - midIndex];
		
		for(int i = 0; i < leftArray.length; i++) {
			leftArray[i] = inputArray[i];
		}
		
		for(int i = midIndex; i < inputArrayLength; i++) {
			//prob1
			rightArray[i - midIndex] = inputArray[i];
		}
		
		//recursive in order to converge prob
		mergeSort(leftArray);
		mergeSort(rightArray);
		
		//merge elements
		merge(inputArray, leftArray,rightArray);
		
		
	}
	
	private static void merge(int[] inputArray, int[] leftArray, int[] rightArray) {
		int leftSize = leftArray.length;
		int rightSize = rightArray.length;
		
		//for leftside pointer, rightside pointer and mergeArray pointer
		int i = 0, j = 0, k = 0;
		
		//start compare and merge
		while(i < leftSize && j < rightSize) {
			if(leftArray[i] <= rightArray[j]) {
				inputArray[k] = leftArray[i];
				i++;
			}else {
				inputArray[k] = rightArray[j];
				j++;
			}
			k++;
		}
		
		//fill remaining elements into inputArray
		while(i < leftSize) {
			inputArray[k] = leftArray[i];
			i++;
			k++;
		}
		
		while(j < rightSize) {
			inputArray[k] = rightArray[j];
			j++;
			k++;
		}
	}
}
```
優點：穩定排序、對於數組的數據集中性較沒有要求<br/>
缺點：需要額外的記憶體空間<br/>

### Bucket Sort
平均時間複雜度：O(n+k)，k為桶數
最壞時間複雜度：O(n^2)
空間複雜度：O(nk)
將數列中的元素分到有限數量的桶子中，再分別對各桶子進行排序，最終再將各桶子結果合併成最終結果。<br/>
1.設定一個定量的陣列當作空桶子。<br/>
2.尋訪序列，並且把項目一個一個放到對應的桶子去。<br/>
3.對每個不是空的桶子進行排序。<br/>
4.從不是空的桶子裡把項目再放回原來的序列中。<br/>
![Bucket_sort_1 svg](https://user-images.githubusercontent.com/89962742/233770086-93ffffcc-1cd6-4cdf-99bf-2075d2d714b5.png)<br/>
![Bucket_sort_2 svg](https://user-images.githubusercontent.com/89962742/233770094-59f1561a-c931-4308-9778-4845047a7191.png)<br/>

```JAVA
public class BucketSort {
	
	private static void bucketSort(int[] inputArray) {
		//traverse inputArray and find the largest element
		int max = inputArray[0];
		for(int i = 0; i < inputArray.length; i++) {
			if(inputArray[i] > max) {
				max = inputArray[i];
			}
		}
		
		//create buckets, the quantity of buckets would be the largest element value plus one
		int[] bucketArray = new int[max + 1];
		//divide diff value of element in inputArray and add count into bucketArray
		for(int i = 0; i < inputArray.length; i++) {
			bucketArray[inputArray[i]]++;
		}
		
		//Reorder the inputArray
		int index = 0;
		for(int i = 0; i < bucketArray.length; i++) {
			for(int j = 0; j < bucketArray[i]; j++) {
				inputArray[index++] = i;
			}
		}
	}
	
	
	public static void main(String[] args) {
		int[] inputArray = new int[100];
		Random random = new Random();
		 
		for(int i = 0; i < inputArray.length; i++) {
			inputArray[i] = random.nextInt(100);
		}
		
		System.out.println("Before:");
		System.out.println(Arrays.toString(inputArray));
		
		bucketSort(inputArray);
		
		System.out.println("After:");
		System.out.println(Arrays.toString(inputArray));
		
	}
}
```
優點：若數據分布均勻，執行速度很快<br/>
缺點：為不穩定排序，需要相對多的記憶體空間<br/>

### Heap Sort
利用堆積這種資料結構所設計的一種排序演算法。堆積是一個近似完全二元樹的結構，並同時滿足堆積的性質：即子節點的鍵值或索引總是小於（或者大於）它的父節點。
平均/最壞/最佳時間複雜度：O(nlogn)
空間複雜度：O(n)
滿足Heap的條件
1.complete binary tree
2.parent > children
heapify - 是指將一個無序的數列轉化為符合堆的性質的過程，也就是將數列中的元素進行重排，構成符合堆的性質的數列。
```JAVA
public class HeapSort {
	
	/* position def
	 * parent = (i - 1) / 2
	 * child1 = 2i + 1
	 * child2 = 2i + 2
	 */
	
	//n:node quantity in this tree; i:heapify with node i 
	private static void heapify(int[] tree, int n, int i) {
		if( i >= n) return;
		int c1 = 2 * i + 1;
		int c2 = 2 * i + 2;
		int max = i;
		//check parent and child node value, put the largest to parent
		if(c1 < n && tree[c1] > tree[max]) {
			max = c1;
		}
		if(c2 < n && tree[c2] > tree[max]) {
			max = c2;
		}
		if(max != i) {
			swap(tree, max, i);
			heapify(tree, n, max);
		}
		
	}
	
	private static void swap(int tree[], int i, int j) {
		int temp = tree[i];
		tree[i] = tree[j];
		tree[j] = temp;
	}
	
	//create a complete binary tree, which all parent node is larger than child nodes
	private static void buildHeap(int[] tree, int n) {
		int lastNode = n - 1;
		int parentNode = (lastNode - 1) / 2;
		for(int i = parentNode; i >= 0; i--) {
			heapify(tree, n, i);
		}
	}
	
	public static void heapSort(int[] tree, int n) {
		buildHeap(tree, n);
		for(int i = n - 1; i >= 0; i--) {
			swap(tree, i, 0);
			heapify(tree, i, 0);
		}
	}
	

	public static void main(String[] args) {
		int[] tree = {4, 2, 8, 3, 6, 1, 9, 7, 5};
		int i = tree.length;
		heapSort(tree, i);
		System.out.println(Arrays.toString(tree));
	}

}
```
優點：需要的時間複雜度穩定為nlogn<br/>
缺點：非穩定排序、需要額外記憶體空間<br/>

### Cocktail Sort
冒泡排序法的變形，在序列中雙向進行排序，故也稱來回排序、快樂小時排序(?。<br/>
![Sorting_shaker_sort_anim](https://user-images.githubusercontent.com/89962742/233787312-49869e09-8732-45cd-b095-c4563d518878.gif)

```JAVA
public class CockTailSort {
	public static void cocktailSort(int[] arr) {
	    int left = 0;
	    int right = arr.length - 1;
	    while (left < right) {
	        // 從左遍歷到右，把比較大的放到右邊
	        for (int i = left; i < right; i++) {
	            if (arr[i] > arr[i + 1]) {
	                int temp = arr[i];
	                arr[i] = arr[i + 1];
	                arr[i + 1] = temp;
	            }
	        }
	        right--;
	        // 從右遍歷到左，把比較小的放到左邊
	        for (int i = right; i > left; i--) {
	            if (arr[i] < arr[i - 1]) {
	                int temp = arr[i];
	                arr[i] = arr[i - 1];
	                arr[i - 1] = temp;
	            }
	        }
	        left++;
	    }
	}

	public static void main(String[] args) {
		int[] inputArray = {3, 1, 5, 2, 9, 7, 4, 6, 8};
		cocktailSort(inputArray);
		System.out.println(Arrays.toString(inputArray));

	}

}
```
優點：比泡沫排序法快一點，聽起來會讓人想小酌一杯<br/>
缺點：O(n^2)有夠慢<br/>

### Leetcode 347. Top K Frequent Elements
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

Example 1:
```JAVA
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```
Example 2:
```JAVA
Input: nums = [1], k = 1
Output: [1]
```

Constraints:

* 1 <= nums.length <= 105
* -104 <= nums[i] <= 104
* k is in the range [1, the number of unique elements in the array].
* It is guaranteed that the answer is unique.

Sol<br/>
<img width="603" alt="image" src="https://user-images.githubusercontent.com/89962742/233777236-56cb1260-804d-4799-acb7-243da01c8fd6.png">


### Leetcode 4. Median of Two Sorted Arrays
Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

Example 1:
```JAVA
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```
Example 2:
```JAVA
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

Constraints:

* nums1.length == m
* nums2.length == n
* 0 <= m <= 1000
* 0 <= n <= 1000
* 1 <= m + n <= 2000
* -106 <= nums1[i], nums2[i] <= 106

Sol.<br/>
<img width="603" alt="image" src="https://user-images.githubusercontent.com/89962742/233784256-fb2ba7f5-76d7-47c2-9171-7a90c759f743.png">


### Bonus Kadane Algorithm  
数列的一维方向找到一个连续的子数列，使该子数列的和最大。例如，对一个数列 −2, 1, −3, 4, −1, 2, 1, −5, 4，其连续子数列中和最大的是 4, −1, 2, 1, 其和为6。
```JAVA
public class KadaneAlgorithm {

	public static void main(String[] args) {
		int[] inputArray = new int[] { (-2), 1, (-3), 4, 1, (-6)};
		int max = maxSubArraySum(inputArray);
		System.out.println(max);
	}
	public static int maxSubArraySum(int[] nums) {
	    int maxSum = nums[0], currentSum = nums[0];
	    for (int i = 1; i < nums.length; i++) {
	        currentSum = Math.max(nums[i], currentSum + nums[i]);
	        maxSum = Math.max(maxSum, currentSum);
	    }
	    return maxSum;
	}
}
```

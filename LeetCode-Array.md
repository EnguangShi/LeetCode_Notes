# 李抠刷题-Array

## 704. Binary Search 二分法-查找目标值并返回下标

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

示例 1:

```text
输入: nums = [-1,0,3,5,9,12], target = 9     
输出: 4       
解释: 9 出现在 nums 中并且下标为 4     
```

示例 2:

```text
输入: nums = [-1,0,3,5,9,12], target = 2     
输出: -1        
解释: 2 不存在 nums 中因此返回 -1 
```

答案：left,right负责标记查找的范围[left, right]。pivot是中间的值，如果大于pivot则往右找，小于pivot则往左找。

```java
class Solution {
    public int search(int[] nums, int target) {
        int pivot;
        int left = 0;
        int right = nums.length - 1;
        while(left <= right) { // the base case is when left == right
            pivot = (left + right) / 2;
            if(nums[pivot] == target){
                return pivot;
            }else if(target > nums[pivot]){
                left = pivot + 1;
            }else{ // target < nums[pivot]
                right = pivot - 1;
            }
        }
        return -1;
    }
}
```

适用范围：An array in ascending or descending order, with no duplicate element.

## 27. Two Pointers双指针-删除Array元素

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并**原地**修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

示例 1: 给定 nums = [3,2,2,3], val = 3, 函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。 你不需要考虑数组中超出新长度后面的元素。

示例 2: 给定 nums = [0,1,2,2,3,0,4,2], val = 2, 函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。

**你不需要考虑数组中超出新长度后面的元素。**

答案：快指针负责iterate所有值。慢指针负责指出新array当前的最后一个待插入位置。当快指针发现不需删除的值时，和慢指针的位置交换值。

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int j = 0; // slow index j -> elements that will stay 
      	// fast index i -> elements that will be detected
        for(int i = 0; i<nums.length; i++){ 
            if(nums[i] != val){ // 每次快指针没找到val
                nums[j] = nums[i]; // 将快指针的值移到慢指针位置
              										 //（保留快指针位置的元素）
                j++; // 增加慢指针和快指针的值
            } // 每次快指针找到val
          		//不将快指针的值移到慢指针位置
          		//（不保留快指针位置的元素）
          		//只增加快指针的值
        }
        return j;
    }
}
```

## 977. Two Pointers双指针-给有序数组的平方排序

给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

示例 1： 输入：nums = [-4,-1,0,3,10] 输出：[0,1,9,16,100] 解释：平方后，数组变为 [16,1,0,9,100]，排序后，数组变为 [0,1,9,16,100]

示例 2： 输入：nums = [-7,-3,2,3,11] 输出：[4,9,9,49,121]

答案：两头最大，中间最小

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int i = 0;  // left index
        int j = nums.length - 1;  // right index
        int[] newNums = new int[nums.length];
        for(int k = newNums.length - 1; k >= 0; k--){
            if(nums[j]*nums[j]>=nums[i]*nums[i]){
                newNums[k]=nums[j]*nums[j];
                j--;
            }else{  // nums[i]*nums[i]>nums[j]*nums[j]
                newNums[k]=nums[i]*nums[i];
                i++;
            }
        }
        return newNums;
    }
}
```

## 209. Two Pointers Sliding Window双指针滑动窗口-寻找长度最小的满足sum>=target的子数组

给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

示例：

输入：s = 7, nums = [2,3,1,2,4,3] 输出：2 解释：子数组 [4,3] 是该条件下的长度最小的子数组。

答案：窗口总和小于target，右边拓宽，大于target，左边缩紧，并判断当前的长度是否是最小。

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;  // left border of the window
        int sum = 0;  // right border of the window
        int result = nums.length + 1;  // return value
        for (int right = 0; right < nums.length; right++){
            sum += nums[right];
            while(sum >= target){
                result = Math.min(result, right - left + 1);
                sum -= nums[left++];  
              	// sum = sum - nums[left]; left++
            }
        }
        return result == nums.length + 1 ? 0 : result;
    }
}
```

注意for套while循环可用作双指针窗口左右端走动。

## 59. Spiral Matrix螺旋矩阵-把数组元素按螺旋填入双层数组

给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

示例:

输入: 3 输出: [ [ 1, 2, 3 ], [ 8, 9, 4 ], [ 7, 6, 5 ] ]

答案：把矩阵分为从外到里一层一层，每层各占两行两列。总共n行n列个元素则会有n/2层（n=3为2层，n=4也为2层，因为如果n是奇数，则n^2是奇数，那么矩阵最中间会有一个元素）。每层可以分为4行（上，右，下，左）。每行左闭右开，都不包括最大的那个元素。定义每层的起始点，按每层套每行循环填入。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] matrix = new int[n][n];
        int layer = n / 2;  // number of layers
        int startX = 0;  // starting position for each layer
        int startY = 0;
        int offset = 1;  
        // the difference between the numbers in a row/column and the numbers in a separation of a layer
        int count = 1;  // the number filled in
        int mid = n / 2;  // the number filled in the middle space
        while (layer > 0) {  // iterate through layers
            int i = startX;
            int j = startY;
          	// 填上面一行
          	// j will finally reach startY + n - offset + 1
            for(; j < startY + n - offset; j++){
               	matrix[i][j] = count++;
            }
          	// 填右边一行
            for(; i < startX + n - offset; i++){
                matrix[i][j] = count++;
            }
          	// 填下面一行
        	  // j starts from startY + n - offset + 1
            for(; j > startY; j--){
                matrix[i][j] = count++;
            }
          	// 填左边一行
            for(; i > startX; i--){
                matrix[i][j] = count++;
            }
            layer--;  // 减少一层
            startX++;  // 起始点往中间移一层
            startY++;  // 起始点往中间移一层
            // 减少一层所以一行少了两个元素，下一层offset+2
          	offset += 2;	
        }
        if (n % 2 == 1){  // if n is odd
          	// the last number should be in the middle space
            matrix[mid][mid] = count;  
        }
        return matrix;
    }
}
```

注意奇数要填最中间一个元素。
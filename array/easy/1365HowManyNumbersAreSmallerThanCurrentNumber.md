### Introduction

Given the array nums, for each nums[i] find out how many numbers in the array are smaller than it. That is, for each nums[i] you have to count the number of valid j's such that j != i and nums[j] < nums[i].  
Return the answer in an array.

### Constraints

0 <= num.length <= 500
0 <= num[i] <= 100

### Examples

```
Input: nums = [8,1,2,2,3]
Output: [4,0,1,1,3]

Input: nums = [6,5,4,8]
Output: [2,1,0,3]
```

### Solutions

- My solution

首先想到的是暴力解法，对原数组中的每一个元素都遍历整个数组统计比它小的元素数量。  
这种解法很好理解，也很容易实现，但是时间复杂度为O(N^2)，过于暴力，不太可取

```java
private int[] smallerNumbersThanCurrent(int[] nums) {
    int[] result = new int[nums.length];
    int count;
    for (int i = 0; i < result.length; i++) {
        count = 0;
        for (int j = 0; j < result.length; j++) {
            if (j != i && nums[j] < nums[i]){
                count++;
            }
        }
        result[i] = count;
    }
    return result;
}
```

- Better solution

由于存在约束条件'0 <= nums[i] <= 100'，所以可以定义一个长度为101的新数组statistic（最大数组下标100），  
将原数组中的值转化为新数组的下标，statistic数组中的值为index出现的次数。
定义count数组存放比下标小的元素数量，显然，对任意的count[i]，必有count[i] = count[i - 1] + (i - 1)出现的次数
nums:     [8,1,2,2,3,3,3,9,9]
statistic:[0,1,2,3,0,0,0,0,1,2,0,0...0]
count:    [0,0,1,3,6,6,6,6,6,7,9,9...9]
result:   [6,0,1,1,3,3,3,7,7]

```java
private int[] smallerNumbersThanCurrent2(int[] nums){
    int[] statistic = new int[101];
    for (int i: nums) {
        statistic[i]++;
    }

    int[] count = new int[101];
    for (int i = 1; i < count.length; i++) {
        count[i] = count[i-1] + statistic[i-1];
    }

    int[] result = new int[nums.length];
    for (int i = 0; i < result.length; i++) {
        result[i] = count[nums[i]];
    }
    return result;
}
```
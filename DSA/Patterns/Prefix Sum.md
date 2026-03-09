The core idea of the Prefix Sum pattern is to create an auxiliary array (`prefixSum[]`)

where each element at index `i` contains the sum of all elements from the start of the array up to index `i`.

**Prefix Sum Array Construction** 

&nbsp;Using Array to store prefixSum 

```java
    private static int[] buildPrefixSum(int[] nums) {
        int arrLength = nums.length;
        int preFixSum[]=new int[arrLength];
        preFixSum[0]=nums[0];
        for(int i=1;i<arrLength;i++){
            preFixSum[i]=preFixSum[i-1]+nums[i];
        }
        System.out.println(Arrays.toString(preFixSum));
        return preFixSum;
    }
```

&nbsp;

**Using Prefix Sum to Calculate Subarray Sum**

```java
public static int getSubarraySum(int[] nums, int left, int right) {
        int[] prefixSum = buildPrefixSum(nums);
        if(left==0) return prefixSum[right];
        return prefixSum[right]-prefixSum[left-1];
    }

```

&nbsp;caller method:

```java
public static void main(String[] args) {
        int nums[]={1,3,4,2,6,8,2};
        int prefixSumArr[]=buildPrefixSum(nums);
        System.out.println(Arrays.toString(prefixSumArr));
        int subarraySum = getSubarraySum(nums, 2, 4);
        System.out.println(subarraySum);
    }
```

**Output**

```bash
[1, 4, 8, 10, 16, 24, 26]
12
```

&nbsp;**Using HashMap to build prefixSum:**

```java
 private static Map<Integer,Integer> buildPrefixSumMap(int[] nums) {
        Map<Integer,Integer> prefixSumMap=new HashMap<>();
        int prefixSum=0;
        for(int i=0;i<nums.length;i++){
            prefixSum+=nums[i];
            prefixSumMap.put(i,prefixSum);
        }
        System.out.println(prefixSumMap);
        return prefixSumMap;
    }

```

&nbsp;

* * *

&nbsp;
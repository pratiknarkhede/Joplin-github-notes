<span style="color: #262626;">Given an array of integers</span> `nums` <span style="color: #262626;">and an integer</span> `target`<span style="color: #262626;">, return</span> *indices of the two numbers such that they add up to `target`*<span style="color: #262626;">.</span>

<span style="color: #262626;">**Input:** nums = \[2,7,11,15\], target = 9  
**Output:** \[0,1\]  
**Explanation:** Because nums\[0\] + nums\[1\] == 9, we return \[0, 1\].</span>

&nbsp;

&nbsp;

```java
public static int[] twoSumOptimized(int[] nums, int target) {
        HashMap<Integer, Integer> numWithIndx = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (numWithIndx.containsKey(complement)) {
                //remember we need to return indices and at key we are storing array element , at value we are storing its index
                return new int[]{i,numWithIndx.get(complement)};
            }
            numWithIndx.put(nums[i], i);
        }
        return new int[]{};
    }
```

&nbsp;

- <span style="color: var(--primary-text);">We create a HashMap `numToIndex` to store numbers and their indices.</span>
    
- <span style="color: var(--primary-text);">We iterate through the array, and for each number `nums[i]`, we calculate its complement `complement = target - nums[i]`.</span>
    
- <span style="color: var(--primary-text);">We check if the complement exists in the HashMap using `numToIndex.containsKey(complement)`.</span>
    
- <span style="color: var(--primary-text);">If it does, we return the indices of the complement and the current number `i`.</span>
    
- <span style="color: var(--primary-text);">If not, we add the current number and its index to the HashMap using `numToIndex.put(nums[i], i)`.</span>
    
- <span style="color: var(--primary-text);">If we reach the end of the array without finding a solution, we return an empty array or throw an exception.</span>
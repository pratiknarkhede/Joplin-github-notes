## **Problem Statement**

- You are a robber planning to rob houses along a street.
- Each house has a certain amount of money.
- Adjacent houses have security systems connected, so if you rob two adjacent houses, the police will be alerted.
- **Goal:** Maximize the total amount of money you can rob without alerting the police.

&nbsp;

## **Key Observations**

1.  **Choice at Each House:**
    
    - For each house, you have two choices:
        1.  **Rob this house:** Add its money to the total collected up to two houses ago (`prev2`).
        2.  **Skip this house:** Take the total collected up to the previous house (`prev`).
2.  **Optimal Substructure:**
    
    - The maximum money collected up to the current house depends on the maximum money collected up to the previous two houses.
3.  **No Need for Extra Space:**
    
    - Instead of storing results for all houses (e.g., using an array), we only need two variables (`prev` and `prev2`) to track the maximum money collected up to the last two houses.

* * *

## **Logic**

### **Step-by-Step Approach**

1.  Initialize:
    
    - `prev2 = 0`: Maximum money collected up to two houses ago.
    - `prev = 0`: Maximum money collected up to the previous house.
2.  Iterate through each house:
    
    - Calculate the maximum money you can collect up to the current house:
        
        `current_max = max(prev, prev2 + current_house_money)`
        
    - Update:
        
        - `prev2 = prev`: Shift "two houses ago" to "one house ago."
        - `prev = current_max`: Update the "previous house" with the current result.
3.  After processing all houses, `prev` contains the final answer.
    
    * * *
    
    ```java
    package com.pratik.thejavajourney.dsa.dynamicprogramming;
    
    public class HouseRobber {
        public static void main(String[] args) {
            int[] nums = {2, 7, 9, 3, 1};
            HouseRobber robber =new HouseRobber();
            int robbedMoney = robber.rob(nums);
            System.out.println(robbedMoney);
        }
    
        public int rob(int[] nums) {
            // at every house we have two choices
            // either to rob the house , this will allow us to take money from this house
            // and previous to previous house
    
            //or skip the house and keep the money collected until previous house
    
            /*
            *
            *
            *             skip      previous_house_money (prev)
            *                     /
            * current_max= max of
            *                     \ this house money + prev2
            *
            *
            *
            * */
    
            int prev=0;
            int prev2=0;
            int current_max=0;
            int n=nums.length;
            for(int house_money : nums){
                current_max= Math.max(prev,  prev2 + house_money);
                // now as we move to next house , house previous to previous becomes previous
                prev2=prev;
                // prev becomes how much max we have gotten till the prev house
                prev=current_max;
    
            }
    
            return current_max;
        }
    }
    
    ```
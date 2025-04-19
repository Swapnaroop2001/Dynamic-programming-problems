# 💡 Dynamic Programming Problems in Java

This repository contains a curated list of **Dynamic Programming (DP) Solutions** for some of the most popular and frequently asked coding interview questions! Perfect for sharpening your skills on platforms like **LeetCode, HackerRank, Codeforces, GFG**, and more!

---

## 📚 Table of Contents

1. [Climbing Stairs 🧗‍♂️](#1-climbing-stairs)
2. [Min Cost Climbing Stairs 💸🧗](#2-min-cost-climbing-stairs)
3. [House Robber 🏠💰](#3-house-robber)
4. [House Robber II 🏠💰🔁](#4-house-robber-ii)
5. [Longest Palindromic Substring 🔁❤️](#5-longest-palindromic-substring)
6. [Palindromic Substrings 🔍❤️](#6-palindromic-substrings)
7. [Decode Ways 🔢➡️🔤](#7-decode-ways)
8. [Coin Change 🪙💱](#8-coin-change)
9. [Maximum Product Subarray ✖️📈](#9-maximum-product-subarray)
10. [Word Break 🔤🧱](#10-word-break)
11. [Longest Increasing Subsequence 📈📏](#11-longest-increasing-subsequence)
12. [Partition Equal Subset Sum ➗🧩](#12-partition-equal-subset-sum)

---

## 1. Climbing Stairs
```java
class Solution {
    int ways=0;
    public int climbStairs(int n) {
        if(n<=1) return 1;
        ways= climbStairs(n-1)+climbStairs(n-2);
        return ways;
    }
}
```
---
## 2. Min Cost Climbing Stairs
```java
public int minCostClimbingStairs(int[] cost) {
        int n=cost.length;

        int[] dp = new int[n];

        dp[0]=cost[0];
        dp[1]=cost[1];

        for (int i = 2; i < n; i++) {
            dp[i] = cost[i] + Math.min(dp[i - 1], dp[i - 2]);
        }
        
        return Math.min(dp[n - 1], dp[n - 2]);
    }
```
---
## 3. House Robber 
```java
class Solution {
    int max=Integer.MIN_VALUE;
    public int rob(int[] nums) {
        int n=nums.length;
        int []dp=new int[n];

        if(n==0) return 0;
        if(n==1) return nums[0];

        dp[0]=nums[0];
        dp[1]=Math.max(nums[0],nums[1]);

        for(int i=2; i<n; i++){
            dp[i]=Math.max( nums[i]+ dp[i-2], dp[i-1]);

        }
        return dp[n - 1];
    }
}
```
---
## 4. House Robber II
```java
class Solution {
    public int rob(int[] nums) {
        int n=nums.length;
        if (n==0) return 0;
        if (n==1) return nums[0];
        if (n==2) return Math.max(nums[0], nums[1]);

        return Math.max(robLinear(nums, 0, n-2), robLinear(nums,1,n-1));
    }

    private int robLinear(int nums[], int start, int end){
        int prev1=0;
        int prev2=0;

        for (int i=start ;i<=end; i++){
            int current= Math.max(nums[i]+prev2, prev1);
            prev2=prev1;
            prev1=current;
        } 
        return prev1;
    }
}
```
---
## 5. Longest Palindromic Substring 
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s== null || s.length()<1 ) return "";

        int start=0, end=0;

        for(int i=0 ; i< s.length(); i++){
            int l1= expandFromCenter(s, i, i);
            int l2= expandFromCenter(s, i, i+1);

            int len= Math.max(l1, l2);

            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }

        return s.substring(start, end + 1);
    }

    private int expandFromCenter(String s,int start, int end ){
        while( start>= 0  && end < s.length() && s.charAt(start)== s.charAt(end)){
            start--;
            end++;
        }
        return end-start-1;
    }
}

```
---
## 6. Palindromic Substrings
```java
class Solution {
    int count=0;
    public int countSubstrings(String s) {
        if (s == null || s.length()==0) return 0; 

        int left=0, right=0;
        for(int i=0; i< s.length(); i++){
            spread(s, i, i);
            spread(s, i, i+1);
        }

        return count;
    }

    private void spread ( String s, int left, int right){
        while(left>=0 && right< s.length() && s.charAt(left)==s.charAt(right)){
            left--;
            right++;
            count++;
        }
    }
}
```
---
## 7. Decode Ways
```java
public int numDecodings(String s) {
        if (s == null || s.length() == 0 || s.charAt(0) == '0') return 0;

        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1;  
        dp[1] = 1; 

        for(int i=2; i<=n; i++){
            int oneDigit = Integer.parseInt(s.substring(i - 1, i));   
            int twoDigit = Integer.parseInt(s.substring(i - 2, i));

            if(oneDigit>=1){
                dp[i]= dp[i]+ dp[i-1];
            }
            if(twoDigit>=10 && twoDigit<=26){
                dp[i]= dp[i]+ dp[i-2];
            }
        }
        return dp[n];
    }
```
---
## 8. Coin Change
```java
public int coinChange(int[] coins, int amount) {
        int n=coins.length;
        int []dp=new int[amount + 1];
        Arrays.fill(dp, amount+1);
        dp[0]=0;
        for(int i=1; i<= amount; i++){
            for(int j=0; j< coins.length; j++){
                if(i-coins[j]<0) continue;
                dp[i]=Math.min (dp[i], dp[i-coins[j]]+1);
            }
        }

        return dp[amount]==(amount +1 )? -1: dp[amount];
    }
```
---
## 9. Maximum Product Subarray
```java
class Solution {
    public int maxProduct(int[] nums) {
        int result=nums[0];

        int currMax=1;
        int currMin=1;

        for (int n :nums){
            if(n==0){   
                currMax=1;
                currMin=1;
                result=Math.max(result, 0);
            }

            int tmp = currMax * n;
            currMax=Math.max(n, Math.max(currMax*n , currMin*n));
            currMin=Math.min(n, Math.min(tmp, currMin*n));
            result = Math.max(result, currMax);
        }

        return result;
    }
}
```
---
## 10. Word Break
```java
public boolean wordBreak(String s, List<String> wordDict) {
        HashSet <String> set= new HashSet<>();
        int maxWordLength = 0;
        for(String word : wordDict){
            set.add(word);
            maxWordLength = Math.max(maxWordLength, word.length());
        }
        int n=s.length();
        boolean [] dp=new boolean[n+1];
        dp[0]=true;

        for(int i=1 ; i<= s.length(); i++){
            for(int j=i; j>= Math.max(0, i - maxWordLength) ;j--){
                if(dp[j] && set.contains(s.substring(j, i))){
                    dp[i]=true;
                    break;
                }
            }
        }
        return dp[n];
    }
```
---
## 11. Longest Increasing Subsequence
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n=nums.length;
        int [] dp=new int[n];
        Arrays.fill(dp,1);

        for(int i=1; i<n; i++){
            for (int j=0; j<i ; j++){
                if(nums[i]>nums[j]){
                    dp[i]=Math.max(dp[i], dp[j]+1);
                }
            }
        }

        int max=1;
        for(int i=0; i<n; i++){
            max=Math.max(max, dp[i]);
        }

        return max;
    }
}
```
---
## 12. Partition Equal Subset Sum
```java
class Solution {
    public boolean canPartition(int[] nums) {
        int totalSum=0;

        for (int num:nums){
            totalSum+=num;
        }

        if(totalSum%2==1) return false;

        int target= totalSum/2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;

        for (int num : nums) {
            // Traverse the dp array from right to left (to avoid reuse of the same number)
            for (int i = target; i >= num; i--) {
                dp[i] = dp[i] || dp[i - num];
            }
        }

        return dp[target];
    }
}
```

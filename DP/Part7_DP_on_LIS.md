### Longest Increasing Subsequence |(DP-41)
```diff
! Note : given an array, any contigous and non-contiguos "ordered" elements are known as subsequence of the given array.
! Method used for subsequence is "pick/took" or "not pick/not took"
 arr1 = [10,9,2,5,3,7,101,18]
 LIS = [2,3,7,101]   len = 4
 LIS = [2,3,7,18]    len = 4
 
 arr2 = [8,8,8]
 LIS = [8]    len = 1 {clearly state that increasing}
 
+ Various subsequence -> print all the Subsequence -> {Recursion, powerset} -> check for increase -> store the longest -> brute way -> 2^n
 "do not work"
 
 Recurrence Method:
 1. Express everything in term of index.
 2. Explore all the possbility.
 3. take the max length take or not take.
 4. Base case
 
+ fun(ind, prev_ind) : prev index allows us to make a decision to pick of not to pick following items
 fun(0,-1) : give me the length of LIS starting from 0 with not previous element.
 fun(3,0) : length of LIS starting from 3rd index, whose prev_ind is 0
 
! Recurrence :   TC - 2^n    SC - O(n) stack space
 fun(ind,prev_ind){
      // Base case
      // run out of the index
+      if(ind == n) return 0;
      
      // take 
+      if(prev_ind == -1 || arr[ind] > arr[prev_ind]){
           take = 1+fun(ind+1,ind);
      }
      
      // not take
+      not_take = 0+fun(ind+1,prev_ind);
      return max(take,not_take);
 
 }
 
! Overlapping subproblem -> Memoization 
 
 ind = [0...n-1]  ,   prev_ind = [-1...n-1]
 now here we are traversing from -1 for prev_ind so we have to change the co ordinates for prev_ind
 -1 , 0 , 1, 2, 3, 4, 5, ...
 0, 1, 2, 3, 4, 5, ...
 
 so dp size will be : [n][n+1]
 
! Memoization    TC : O(n*n)   SC : O(n*n) + O(2*n) auxiliary stack space 
! passing by refernce of an array or string is necessary it may cause TLE
class Solution {
public:
    int fun(int ind,int prev_ind, vector<int> &nums, vector<vector<int>> &dp,int n){
        // base case
        // if ind exhaust
+        if(ind == n)    return 0;
     
        if(dp[ind][prev_ind+1] != -1) return dp[ind][prev_ind+1];
        
        // not take case
+        int len = 0 + fun(ind+1, prev_ind,nums,dp, n); // len of subsequnce remain same as prev

        // take case
+       if(prev_ind == -1 || nums[ind] > nums[prev_ind]){
            len = max(len,1 + fun(ind+1, ind, nums, dp, n)); // len of subsequnce
        }
        
        
        return dp[ind][prev_ind+1] = len; 
    }
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
+        vector<vector<int>> dp(n,vector<int>(n+1,-1));
+        return fun(0,-1,nums, dp,n);
    }
};
 
 
! Tabulation Approach :
// Base Case
// now ind range to  n-1   ...    0
// prev_ind range to  ind-1  ...   -1  
// copy the recurence and follow coordinate shift

 int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
+        vector<vector<int>> dp(n+1,vector<int>(n+1,0));
+        for(int ind=n-1;ind>=0;ind--){
+            for(int prev_ind=ind-1;prev_ind>=-1;prev_ind--){

                // not take case
+               int len = 0 + dp[ind+1][prev_ind+1];  // len of subsequnce remain same as prev

                // take case
                if(prev_ind == -1 || nums[ind] > nums[prev_ind]){
+                   len = max(len, 1+ dp[ind+1][ind+1]);    // len of subsequnce
                }

+                dp[ind][prev_ind+1] = len; 
            }
        }
    
+        return dp[0][-1+1];
     
    }


! Optimized Solution :     TC : O(n*n)     SC : O(n*2)
   
   int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
+        vector<int> next(n+1,0), curr(n+1,0);
        for(int ind=n-1;ind>=0;ind--){
            for(int prev_ind=ind-1;prev_ind>=-1;prev_ind--){

                // not take case
+                int len = 0 + next[prev_ind+1];  // len of subsequnce remain same as prev

                // take case
                if(prev_ind == -1 || nums[ind] > nums[prev_ind]){
+                   len = max(len, 1+ next[ind+1]);    // len of subsequnce
                }

+                curr[prev_ind+1] = len; 
            }
+            next = curr;
        }
    
+        return curr[-1+1];
     
    }

! More Optimized Solution :     TC : O(n*n)     SC : O(n)

dp[i] - signifies the longest increasing Subsequnece that ends at index i till now.

Items = [5,4,11,1,16,8]

so, dp = [1,1,2,1,3,2]

Method : 

First step : dp[1,1,1,1,1,1]    { Every element has atleast one LIS i.e. itself }
Second step : Look from left to right and check if prev item less than curr one, if it is than curr = max(curr, 1 + prev) { which is less than curr }
          
          dp[1,1,2,1,3,2]

class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
+        vector<int> dp(n,1);
        int ans = 1;
        for(int i=0;i<n;i++){
            for(int prev=0;prev<i;prev++){
+                if(nums[i] > nums[prev]){
+                    dp[i] = max(dp[i],1+dp[prev]);
                }
                
            }   
+            ans = max(ans , dp[i]); 
        }
        return ans;       
    }
};

``` 
 
### Printing the LIS | DP(42)

```diff


! Printing the LIS            TC : O(N*N)    SC : O(N)

#include <bits/stdc++.h>
using namespace std;
int longestIncreasingSubsequence(int nums[], int n)
{

        int maxi = 1;
        int lastIndex = 0;
        vector<int> dp(n,1), hash(n);
        for(int i=0;i<n;i++){
            hash[i] = i;
            for(int prev=0;prev<i;prev++){
+                if(nums[i] > nums[prev]  && 1+dp[prev] > dp[i]){
+                    dp[i] = 1+ dp[prev];
+                    hash[i] = prev;
                }
                
            }   

            if(dp[i] > maxi){
                maxi = dp[i];
                lastIndex = i;
            }

        }
        vector<int> ans;
+        ans.push_back(nums[lastIndex]);
+        while(lastIndex != hash[lastIndex]){
            
+            lastIndex = hash[lastIndex];
+            ans.push_back(nums[lastIndex]);
        }

+        reverse(ans.begin(), ans.end());
        for(int i=0;i<ans.size();i++){
+            // cout << ans[i] << " ";   // this line print the LIS Sequence
        }
        return ans.size();
        
}

```

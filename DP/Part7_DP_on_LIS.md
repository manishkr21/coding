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
      if(ind == n) return 0;
      
      // take 
      if(prev_ind == -1 || arr[ind] > arr[prev_ind]){
           take = 1+fun(ind+1,ind);
      }
      
      // not take
      not_take = 0+fun(ind+1,prev_ind);
      return max(take,not_take);
 
 }
 
 ! Overlapping subproblem -> Memoization 
 
 ind = [0...n-1]  ,   prev_ind = [-1...n-1]
 now here we are traversing from -1 for prev_ind so we have to change the co ordinates
 -1 , 0 , 1, 2, 3, 4, 5, ...
 0, 1, 2, 3, 4, 5, ...
 
 
``` 

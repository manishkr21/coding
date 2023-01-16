

```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```

### Longest Common String
```diff
#include <bits/stdc++.h> 
int lcs(string &str1, string &str2){
  int n=str1.size();
  int m=str2.size();
  vector<vector<int>> dp(n+1,vector<int>(m+1,0));
  int ans = 0;
  for(int i=1;i<=n;i++){
      for(int j=1;j<=m;j++){
          if(str1[i-1] == str2[j-1]){
+             dp[i][j] = 1 + dp[i-1][j-1];
+             ans = max(ans, dp[i][j]);
          }
          else{
+              dp[i][j] = 0;
          }
      }
  }
  return ans;

}
```
### Longest Common Palindrome
```diff   
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        string s1 = s;
        reverse(s.begin(),s.end());
        cout << s1 << " " << s;
        int n = s.size();
        vector<int> prev(n+1,0), curr(n+1,0);
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
+                if(s1[i-1] == s[j-1]) curr[j]=1+prev[j-1];
+                else curr[j] = max(prev[j],curr[j-1]);
            }
+            prev = curr;
        }
        return prev[n];
    }
};
```

### Minimum insertion to make a string palindrome
```diff
S = "abc"
! You can insert any character anywhere in the string
! to make it palindrome just add reverse of it , but wwould take O(length of string) operation, we need to reduce it
Procedure:
  1. find longest common palidrome
  2. keep it intact
  3. condingninjas       --->   palindrome {ingni}
  4. cond {sajn} ingni njas {donc}    --->  if the length of string is n
+ 5. ans = n - size of longest common palindrome 

class Solution {
public:

    int minInsertions(string s) {
        string r = s;
        reverse(s.begin(), s.end());
        int n=s.size();
        vector<vector<int>> dp(n+1,vector<int>(n+1,0));
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(s[i-1]== r[j-1]){
                    dp[i][j] = 1+dp[i-1][j-1];
                }else{
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }

+       int len = dp[n][n];
+       return n-len;
    }
};

```


### Minimum insertion/deletion to convert a string A to string B
```diff
 A= 'abcd'  B='anc'
! abcd {two deletions}  ->  ac {one insertion}(LCS)  -> anc
+ number of deletion = len of string A - size of LCS
+ number of insertion = len of string B - size of LCS

class Solution{
	public:
	int minOperations(string str1, string str2) 
	{ 
	    int n = str1.size();
	    int m = str2.size();
	    vector<vector<int>> dp(n+1,vector<int>(m+1,0));
	    for(int i=1;i<=n;i++){
		for(int j=1;j<=m;j++){
		    if(str1[i-1]==str2[j-1]) dp[i][j] = 1+ dp[i-1][j-1];
		    else dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
		}
	    }
+	    int deletions = n - dp[n][m];
+	    int insertions =  m - dp[n][m];
	    return insertions + deletions;
	} 
};

```

### Shortest Common Supersequence
```diff
S1 = 'brute'   S2 = 'groot'   , supersequence -> 'brutegroot'          
It is shortest ? ans is no => shortest supersequence "bgruoote"

S1='bleed'     S2='blue'      Shortest Supersequence -> 'bleued'
 
ans = size(LCS) + size(S1) - size(LCS) + size(S2) - size(LCS) 
+ so, ans = n+m-size(LCS)

Procedure:
1. Take common guys single time
2. len of shortest common supersquence = size(s1) + size(s2) - len(LCS)

! see Example -
	    g r o o t 
	  0 1 2 3 4 5
	0 0 0 0 0 0 0  
   b	1 0 0 0 0 0 0
   r	2 0 0 1 1 1 1
   u	3 0 0 1 1 1 1
   t	4 0 0 1 1 1 2
   e	5 0 0 1 1 1 2

```

![All for IMage](https://raw.githubusercontent.com/manishkr21/coding/main/dp-array-lcs2.png)

```diff
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int n = size(str1);
        int m = size(str2);
        vector<vector<int>> dp(n+1,vector<int>(m+1,0));
        
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(str1[i-1] == str2[j-1]) dp[i][j] = 1+ dp[i-1][j-1];
                else dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
            }
        }

        int i=n,j=m;
        string res = "";
+       int len = n+m-dp[i][j];    // length of supersequence
   
        while(i>0 && j>0){
            if(str1[i-1] == str2[j-1]){
                res+=str1[i-1];
                i--;j--;
            }else if(dp[i-1][j] > dp[i][j-1]){
+              res+=str1[i-1];
                i--;
            }else{
+              res+=str2[j-1];
                j--;
            }
        }
+      while(i>0){ res += str1[i-1]; i--; }
+      while(j>0){ res += str2[j-1]; j--; }

+      reverse(res.begin(),res.end());
       return res;
    }
};
```

### Distinct Supersequence of S2 in S1
```diff
	S1 = 'babgbag'  S2 = 'bag'
	HINT: Trying all ways  -> Recursion
	Count the number of ways : Sum them
	
! Procedure to write Recurrence:
	1. Express everything in term of (i,j).
	2. Explore all possiblities.
	3. Return summation of all possibilies.
	4. base case.

! Recursion: TC - exponential   {2^n * 2^m}   SC - O(N+M)
fun(i,j){
	if(j<0) return 1;    // match found
	if(i<0) return 0;    // still some char in S2 remain to match
	if(s1[i] == s2[j]){
		return (fun(i-1,j-1) + fun(i-1,j)); // match + ignore match
	}else{
		return fun(i-1,j);    // just skip first element from the first one  
	}
}
 
! Memoization:  TC - O(n*m)   SC - O(n*m) + O(n+m)   // dp + Auxiliary stack space

class Solution {
public:
    int fun(string s, string t,int i,int j, vector<vector<int>> &dp){
        if(j<0) return 1;
        if(i<0) return 0;
        if(dp[i][j] != -1) return dp[i][j];
+       if(s[i] == t[j]) return dp[i][j] = fun(s,t,i-1,j-1,dp)+fun(s,t,i-1,j,dp);
+        return dp[i][j] = fun(s,t,i-1,j,dp);
    }
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
        
        vector<vector<int>> dp(n+1,vector<int>(m+1,-1));
        
        return fun(s,t,n-1,m-1,dp);
    }
};

! Tabulation: TC - 
! because above range is -1 to n thus below add 1 to each term
! now if j==0 return 1,   and if i==0 return 0
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
+       vector<vector<double>> dp(n+1,vector<double>(m+1,0));
        for(int i=0;i<=n;i++) dp[i][0] = 1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
+               if(s[i-1]==t[j-1]) dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
+               else dp[i][j] = dp[i-1][j];
            }
	}
	
        return (int)dp[n][m];
    }
};

! More optimized approach
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
+       vector<double> prev(m+1,0), curr(m+1,0);
+        curr[0] = prev[0] = 1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
               if(s[i-1]==t[j-1]) curr[j] = prev[j-1] + prev[j];
               else curr[j] = prev[j];
            }
+	    prev = curr;
	}
	
        return (int)prev[m];
    }
};

```
![All for IMage](https://raw.githubusercontent.com/manishkr21/coding/main/distinct-subseq.png)

```diff
! One Array optization
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
        vector<double> prev(m+1,0);
        prev[0] = 1;
        for(int i=1;i<=n;i++){
            for(int j=m;j>=1;j--){
+               if(s[i-1]==t[j-1]) prev[j] = prev[j-1] + prev[j];             
            }
	}
	
        return (int)prev[m];
    }
};


```

### Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.
You have the following three operations permitted on a word:
- Insert a character
- Delete a character
- Replace a character <br>

![All for IMage](https://raw.githubusercontent.com/manishkr21/coding/main/edit-distance.png)
![All for IMage](https://raw.githubusercontent.com/manishkr21/coding/main/edit-distance2.png)
```diff
! Example : word1 = "horse", word2 = "ros"

```


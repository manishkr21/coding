
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
+	    int insertions = n - dp[n][m];
+	    int deletions =  m - dp[n][m];
	    return insertions + deletions;
	} 
};

```

### Shortest Common Supersequence
```diff
S1 = 'brute'   S2 = 'groot'   , supersequence -> 'brutegroot'          
Is is shortest ? ans is no => shortest supersequence "bgruoote"

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


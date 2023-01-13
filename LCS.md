
```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```

### --------------------------------------Longest Common String ------------------------------------
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
              dp[i][j] = 1 + dp[i-1][j-1];
              ans = max(ans, dp[i][j]);
          }
          else{
+              dp[i][j] = 0;
          }
      }
  }
  return ans;

}
```
### --------------------------------Longest Common Palindrome-------------------------------------
   
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
                        if(s1[i-1] == s[j-1]) curr[j]=1+prev[j-1];
                        else curr[j] = max(prev[j],curr[j-1]);
                    }
                    prev = curr;
                }
                return prev[n];
            }
        };

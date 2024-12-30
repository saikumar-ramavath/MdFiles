### # of palindromes in a string
- expanding center approach 
- dp approach with first setting all len-1 string to true, len-2 string to true, and from len 3 check ith == jth(len 3rd) & ith+1 and jth-1 to be true, then it is palindrome

```
void computePalindromeTable(string &s) {
        int n = s.size();
        isPalindrome.resize(n, vector<bool>(n, false));
        for (int i = 0; i < n; ++i) {
            isPalindrome[i][i] = true;
        }
        for (int len = 2; len <= n; ++len) {
            for (int i = 0; i <= n - len; ++i) {
                int j = i + len - 1;
                if (len == 2) {
                    isPalindrome[i][j] = (s[i] == s[j]);
                } else {
                    isPalindrome[i][j] = (s[i] == s[j]) && isPalindrome[i + 1][j - 1];
                }
            }
        }
    }
```
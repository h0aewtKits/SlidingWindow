# SlidingWindow

核心步骤
- right 右移
- 收缩
- left 右移
- 求结果

>[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)
>
> return the minimum window substring of s such that every character in t (including duplicates) is included in the window.
>
>Input: s = "ADOBECODEBANC", t = "ABC" 
>
>Output: "BANC"
>
>Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] winMap = new int[256];
        int[] tMap = new int[256];
        int charNum = 0;

        for(char ele: t.toCharArray()){
            if(tMap[ele] == 0) charNum++;
            tMap[ele]++;
        }

        int left = 0, right = 0, match = 0, start = 0, end = 0;
        int minLen = Integer.MAX_VALUE;

        while(right < s.length()){
            char rightChar = s.charAt(right);
            
            if(tMap[rightChar] != 0){ //注意这个判断，必须要有
                winMap[rightChar]++;

                if(winMap[rightChar] == tMap[rightChar]) match++; //will be 3 if t is AABC; match for A will be reach 2 then match++
            }
            right++;

            while(match == charNum){
                if(right - left < minLen){
                    start = left;
                    end = right;
                    minLen = end - start;
                }
                char leftChar = s.charAt(left);

                if(tMap[leftChar] != 0){ //注意这个判断，必须要有 
                    if(winMap[leftChar] == tMap[leftChar]) match--;  //（和上面是对称结构）
                    winMap[leftChar]--;
                }
                left++;
            }
        }
        if(minLen == Integer.MAX_VALUE) return "";
        return s.substring(start, end);
    }
}
```

>[567. Permutation in String](https://leetcode.com/problems/permutation-in-string/description/)
>
>return true if one of s1's permutations is the substring of s2.
>
>Input: s1 = "ab", s2 = "eidbaooo"
>
>Output: true
>
>Explanation: s2 contains one permutation of s1 ("ba").

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int[] winMap = new int[255];
        int[] map = new int[255];
        int charNum = 0;
        
        for(char c: s1.toCharArray()){
            if(map[c] == 0) charNum++;
            map[c]++;
        }
        System.out.println(charNum);
        
        int left = 0; int right = 0;
        int match = 0;
        
        while(right < s2.length()){
            char rightChar = s2.charAt(right);
            if(map[rightChar] != 0){
                winMap[rightChar]++;
                if(winMap[rightChar] == map[rightChar]){
                    match++;
                }
            }
            right++;

            while(match == charNum){
                char leftChar = s2.charAt(left);
                
                if(map[leftChar] != 0){
                    if(winMap[leftChar] == map[leftChar]){
                        match--;
                    }
                    winMap[leftChar]--;
                }
                
                if(right - left == s1.length()){

                    return true;
                }
                left++;
            }
        }
        return false;
    }
}
```

[438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)

>Given two strings s and p, return an array of all the start indices of p's anagrams in s. 
>
>Input: s = "cbaebabacd", p = "abc"
>
>Output: [0,6]
>
>Explanation:
>
>The substring with start index = 0 is "cba", which is an anagram of "abc".
>
>The substring with start index = 6 is "bac", which is an anagram of "abc".
>

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList<>();
        
        int[] pMap = new int[255];
        int[] winMap = new int[255];
        int charNum = 0;
        
        for(char c: p.toCharArray()){
            if(pMap[c] == 0){
                charNum++;
            }
            pMap[c]++;
        }
        
        int left = 0; int right = 0;
        int match = 0;
        
        while(right < s.length()){
            char rightChar = s.charAt(right);
            if(pMap[rightChar] != 0){
                winMap[rightChar]++;
                
                if(winMap[rightChar] == pMap[rightChar]){
                    match++;
                }
            }
            right++;
            
            while(charNum == match){
                char leftChar = s.charAt(left);
                
                if(pMap[leftChar] != 0){
                    
                    if(pMap[leftChar] == winMap[leftChar]){
                        match--;    
                    }
                    winMap[leftChar]--;
                }
                if(right - left == p.length()){
                    res.add(left);
                }
                
                left++;
            }
            
        }
        return res;
        
    }
}
//https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/92007/Sliding-Window-algorithm-template-to-solve-all-the-Leetcode-substring-search-problem.
```

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

>Given a string s, find the length of the longest substring without repeating characters.
>
>Input: s = "abcabcbb"
>
>Output: 3
>
>Explanation: The answer is "abc", with the length of 3.
>

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length() == 0) return 0;
        int[] map = new int[255];

        int left = 0, right = 0;
        int maxLen = Integer.MIN_VALUE;
        while(right < s.length()){
            char rightChar = s.charAt(right);
            map[rightChar]++;
            right++;

            while(map[rightChar] > 1){
                char leftChar = s.charAt(left);
                map[leftChar]--;
                left++;
            }
            System.out.println(s.substring(left, right));
            /*
                a
                ab
                abc
                bca
                cab
                abc
                cb
                b
            */
            maxLen = Math.max(maxLen, right - left);
        }
        return maxLen;
    }
}
```

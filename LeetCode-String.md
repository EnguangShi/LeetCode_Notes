# 李抠刷题-String

## 344. Two Pointers双指针-反转字符串

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

示例 1：
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]

示例 2：
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]

答案：用双指针两头交换，再一起往中间走。

```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0;
        int r = s.length - 1;
        while (l < r) {
          	// 交换a和b:a=a^b, b=a^b = a^b^b=a, a=a^b = a^b^a=b
        		// 构造a^b的结果，并放在a中
            s[l] ^= s[r];  
            // 将a^b这一结果再^b，存入b中此时b=a, a=a^b
            s[r] ^= s[l];  
            // a^b的结果再^a，存入a中，此时b=a, a=b 完成交换
            s[l] ^= s[r];  
            l++;
            r--;
        }
    }
}
```

## 541. Jump Iteration跳跃遍历-按间隔反转字符串

给定一个字符串 s 和一个整数 k，你需要对从字符串开头算起的每隔 2k 个字符的前 k 个字符进行反转。

如果剩余字符少于 k 个，则将剩余字符全部反转。

如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

示例:

输入: s = "abcdefg", k = 2
输出: "bacdfeg"

答案：每2k个元素遍历一次，反转2k个元素开头的k个元素（使用上题方法）。

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] arr = s.toCharArray();
        for(int i = 0; i < arr.length; i += 2*k){
            int left = i;
          	// 判断right位置是array终点还是k个char的末端
            int right = Math.min(i + k - 1, arr.length-1);
            while (left < right){
            		// 这里也可用上题的^=交换
                char tmp = a[left];
                a[left++] = a[right];
                a[right--] = a[left];
            }
        }
        String str = String.valueOf(arr);
        return str;
    }
}
```

第一题的while loop可以放到这种题里作为小snipplet用。

## 剑指5. StringBuilder-替换String中的空格为其他东西

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

示例 1： 输入：s = "We are happy."
输出："We%20are%20happy."

答案：新建一个StringBuilder，把原来的String中一个个字母放进去

```java
class Solution {
    public String replaceSpace(String s) {
      	// sb: mutable String (sb.append直接在sb上进行append, 而不用赋给一个新的String)
        StringBuilder sb = new StringBuilder();  
        for(int i = 0; i < s.length(); i++){
         		// s.charAt(<index>):提取string中的char, 类似提取array中的元素
          	// String.valueOf():把东西转换为String
            if(String.valueOf(s.charAt(i)).equals(" ")){  
                sb.append("%20");
            }else{
              	// StringBuilder可以直接append Char
                sb.append(s.charAt(i));  
            }
        }
      	// StringBuilder需要用toString()转换成String
        return sb.toString();  
    }
}
```

## trim, split, \\\s+方法运用-把字符串里的单词从尾到头放

给定一个字符串，逐个翻转字符串中的每个单词。

示例 1：
输入: "the sky is blue"
输出: "blue is sky the"

示例 2：
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

示例 3：
输入: "a good  example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

答案：

```java
class Solution {
    public String reverseWords(String s) {
        String trimedString;
      	// trim()可以去掉左右两边所有的空格
        trimedString = s.trim();
      	// \\s+ is multiple spaces
        String[] splitString = trimedString.split("\\s+");  
        StringBuilder sb = new StringBuilder();
      	// String Array length不能有括号
        for(int i = splitString.length - 1; i >= 0 ; i--){
            sb.append(splitString[i]);
            if(i != 0) {
                sb.append(" ");
            }
        }
        return sb.toString();
    }
}
```


---
title: 17th Jan HJ5 进制转换
date: 2023-03-17 23:28:36
---

```class Solution {
写出一个程序，接受一个十六进制的数，输出该数值的十进制表示。

数据范围：保证结果在 1 \le n \le 2^{31}-1 \1≤*n*≤231−1 

##### 输入描述：

输入一个十六进制的数值字符串。

##### 输出描述：

输出该数值的十进制字符串。不同组的测试用例用\n隔开。

```
输入：
0xAA
输出：
170
```

```
import java.util.Scanner;
import java.lang.Math;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int count = 0;
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNextLine()) {
            String num = in.nextLine();
            for (int i = 2; i < num.length(); i++) {
                char hex_num = num.charAt(i);
                int dec_num = 0;
              //一位一位的取
                if (hex_num >= '0' && hex_num <= '9') {
                    dec_num = hex_num - '0';
                } else if (hex_num >= 'A' && hex_num <= 'F') {
                    dec_num = hex_num - 'A' + 10;
                } else if (hex_num >= 'a' && hex_num <= 'f') {
                    dec_num = hex_num - 'a' + 10;
                }
                count += dec_num * Math.pow(16, num.length() - i - 1);

            }
        }
        System.out.println(count);
    }
}
```

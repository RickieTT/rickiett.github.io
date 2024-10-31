---
title: 25th 删除字符串中出现次数最少的字符
date: 2023-03-20 23:41:23
---

#### 25th 删除字符串中出现次数最少的字符

开发一个坐标计算工具， A表示向左移动，D表示向右移动，W表示向上移动，S表示向下移动。从（0,0）点开始移动，从输入字符串里面读取一些坐标，并将最终输入结果输出到输出文件里面。

输入：

合法坐标为A(或者D或者W或者S) + 数字（两位以内）

坐标之间以;分隔。

非法坐标点需要进行丢弃。如AA10; A1A; $%$; YAD; 等。

下面是一个简单的例子 如：

A10;S20;W10;D30;X;A1A;B10A11;;A10;

处理过程：

起点（0,0）

A10  = （-10,0）

S20  = (-10,-20)

W10 = (-10,-10)

D30 = (20,-10)

x  = 无效

A1A  = 无效

B10A11  = 无效

一个空 不影响

A10 = (10,-10)

结果 （10， -10）

```
import java.util.*;
import java.io.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) throws IOException{
            int x = 0;
            int y = 0;
            BufferedReader corStr = new BufferedReader(new InputStreamReader(System.in));
            String[] corArray = corStr.readLine().split(";");
            int corArray_len = corArray.length;
        for(int i = 0; i < corArray_len; i++){
            if(corArray[i].length() > 3 || corArray[i].length() == 0){
                continue;
            }
            if(corArray[i].charAt(0) == 'A'){
                if('0' <= corArray[i].charAt(1) &&
                        corArray[i].charAt(1) <= '9'){
                    if(corArray[i].length() == 2){
                        x -= (corArray[i].charAt(1) - '0');
                        continue;
                    }else if('0' <= corArray[i].charAt(2) &&
                            corArray[i].charAt(2) <= '9'){
                        x -= (corArray[i].charAt(1) - '0') * 10;
                        x -= corArray[i].charAt(2) - '0';
                    }


                }

            }else if(corArray[i].charAt(0) == 'D'){
                if('0' <= corArray[i].charAt(1) &&
                        corArray[i].charAt(1) <= '9'){
                    if(corArray[i].length() == 2){
                        x += corArray[i].charAt(1) - '0';
                        continue;
                    }
                    if('0' <= corArray[i].charAt(2) &&
                            corArray[i].charAt(2) <= '9'){
                        x += (corArray[i].charAt(1) - '0') * 10;
                        x += corArray[i].charAt(2) - '0';
                    }

                }
            }else if(corArray[i].charAt(0) == 'W'){
                if('0' <= corArray[i].charAt(1) &&
                        corArray[i].charAt(1) <= '9'){
                    if(corArray[i].length() == 2){
                        y += corArray[i].charAt(1) - '0';
                        continue;
                    }
                    if('0' <= corArray[i].charAt(2) &&
                            corArray[i].charAt(2) <= '9'){
                        y += (corArray[i].charAt(1) - '0') * 10;
                        y += corArray[i].charAt(2) - '0';
                    }

                }
            }else if(corArray[i].charAt(0) == 'S'){
                if('0' <= corArray[i].charAt(1) &&
                        corArray[i].charAt(1) <= '9'){
                    if(corArray[i].length() == 2){
                        y -= corArray[i].charAt(1) - '0';
                        continue;
                    }
                    if('0' <= corArray[i].charAt(2) &&
                            corArray[i].charAt(2) <= '9'){
                        y -= (corArray[i].charAt(1) - '0') * 10;
                        y -= corArray[i].charAt(2) - '0';
                    }

                }
            }else{
                continue;
            }
        }
        System.out.println(x + "," + y);
    }
}
```

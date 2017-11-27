# WriteUps-reference

# Index
* [AIS3 2017 pre-exem](#AIS3-2017-pre-exem)
* [TUCTF 2017](#TUCTF_2017)

## AIS3-2017-pre-exem
* [crypto1 (XOR)](#crypto1)
### crypto1
****
Author:TastyFeeder
Source:[TastyFeederの世界線](http://www.tastyfeeder.com/2017/07/ctf-ais3-pre-exam-2017-writeup.html)
****
題目：
```c=
#include <stdio.h>
#include <string.h>

int main() {
 int val1 = ?????????, val2 = ?????????, val3 = ???????, val4 = ??????, i, *ptr;
 char flag[29] = "????????????????????????????"; // Hint: The flag begins with AIS3
 
 for(i = 0, ptr = (int*)flag ; i < 7 ; ++i)
  printf("%d\n", ptr[i] ^ val1 ^ val2 ^ val3 ^ val4);
 
 /*
 964600246
 1376627084
 1208859320
 1482862807
 1326295511
 1181531558
 2003814564
 */
 
 return 0;
}
```
Solution：  
把flag跟一些東西XOR後，以整數輸出  
簡單的XOR，寫個c來解，先猜開頭是ais3或AIS3就可以知道val1~val4 xor 的值了
```c=
#include <stdio.h>

int main() {
    int result[] ={964600246,1376627084,1208859320,1482862807,1326295511,1181531558,2003814564};
    char flag[29];
    flag[0] = 'A';
    flag[1] = 'I';
    flag[2] = 'S';
    flag[3] = '3';
    int *ptr;
    ptr = (int*)flag;
    int key = result[0] ^ ptr[0];
    int i ;
    for(i = 1, ptr = (int*)flag ; i < 7 ; ++i)
        ptr[i] = key ^ result[i];
    flag[28] = '\0';
    printf("%s\n",flag);
    return;
}    
```

## TUCTF_2017
* [GitGud (.git visible)](#GitGud)
* [Cookie Harrelson (cookie modify&bypass)](#Cookie_Harrelson)
* [iFrame and Shame (php code inject)](#iFrame_and_Shame)

### GitGud
****
Author:k3ramas
Source:[k3ramas.blogspot.tw](http://k3ramas.blogspot.tw/2017/11/tuctf-2017-web-challenges.html)
****
/.git可以看到gitlog 可以從gitlog翻出flag  
算是學到除了robots.txt以外，還有甚麼哪裡可能翻出敏感檔案的一種方式  
flag告訴你：<font color=red>Don't make git public</font>  
要避免這樣的情況，應該要把.git設成403，或是把.git加到.gitignore裡面  
等把git弄熟一點再回來把WriteUp寫的詳細一點

### Cookie_Harrelson
第1次造訪網站，cookie會是 "cat index.txt"  
這個時候重新整理網頁(第2次)，然後用burp去改cookie成"cat flag.txt"  
但是response回來的cookie:"cat index.txt #cat flag.txt"  
可以看到他把flag.txt註解掉了，最後解法居然是換行...= =  
flag告訴你：<font color=red>Don't execute from cookie</font>  

### iFrame_and_Shame
代碼注入，單純考驗你思維夠不夠猥瑣ㄅ  
居然可以從  
```php=
"; echo $(ls) #
```
看到目錄下檔案，真的是666  

不過用  
```
"; echo $(cat flag) #
```
可以看到flag，應該也牽扯到一點SSRF了吧，畢竟client被403了阿？

# Index

* [AIS3 2017 pre-exem](#AIS3-2017-pre-exem)


## AIS3-2017-pre-exem
* [crypto1 (XOR)](#crypto1)

## crypto1
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

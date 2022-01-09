<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>



@[TOC]

# <font color=#6495ED size=6 >Cipher Code</font>

* ### <font color=#000000 size=4 face=粗体>列举Caesar cipher的26种可能</font>

  ```python
  s = input().lower()
  for i in range(0, 26):
      ss = list()
      for j in s:
          if j.isalpha():
              xx = (((ord(j) + i) - ord('a')) % 26) + ord('a')
              ss.append(chr(xx))
          else:
              ss.append(j)
      print(str(i) + ":" + "".join(ss))
  ```

* ### <font color=#000000 size=4 face=粗体>查看Vigenère cipher字符串长度</font>

  ```python
  print(len(list(filter(lambda x : x.isalpha(), list(input())))))
  ```

* ### <font color=#000000 size=4 face=粗体>参照表</font>

  ![img](https://images2015.cnblogs.com/blog/581342/201701/581342-20170104092050112-1556866491.png)

  


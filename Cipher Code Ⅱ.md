<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

@[TOC]

# <font color=#6495ED size=6 >Cipher Code Ⅱ</font>

* ### <font color=#000000 size=4 face=粗体>求是否满足替换密码</font>

  输入为明文和密文，问$key$是否满足条件

  ```python
  '''
  Author: NEFU AB-IN
  Date: 2021-12-18 20:54:00
  FilePath: \DCAS\substitution.py
  LastEditTime: 2021-12-18 21:40:04
  '''
  
  
  def printD(d):
      print(list(d.keys()))
      print(list(d.values()))
  
  
  def solve():
      # plaintext = input("Please input the plaintext: ")
      plaintext = "VACCINATION IMMUNISES"
      ciphertext = input("Please input the ciphertext: ")
      d = dict()
      cnt_cipher = dict()
      for i in range(ord("A"), ord("Z") + 1):
          d.setdefault(chr(i), '0')
          cnt_cipher.setdefault(chr(i), 0)
      for i in range(len(plaintext)):
          if not plaintext[i].isalpha():
              continue
          if d[plaintext[i]] != '0' and d[plaintext[i]] != ciphertext[i]:
              print("There exists collsion")
              print(
                  f"{plaintext[i]} -> old : {d[plaintext[i]]} , new : {ciphertext[i]}"
              )
          if d[plaintext[i]] == '0':
              if cnt_cipher[ciphertext[i]] != 0:
                  print(f"There exists collsion on cipher {ciphertext[i]}")
          cnt_cipher[ciphertext[i]] = 1
          d[plaintext[i]] = ciphertext[i]
      printD(d)
      return
  
  
  if __name__ == "__main__":
      solve()
  ```

  

* ### <font color=#000000 size=4 face=粗体>求AES的state的初始值和Round0后的值</font>

  输入为明文的ASCII码值

  ```python
  '''
  Author: NEFU AB-IN
  Date: 2021-12-15 22:17:33
  FilePath: \DCAS\state.py
  LastEditTime: 2021-12-15 23:26:58
  '''
  
  
  def ex0(x):
      if len(x) <= 1:
          x = '0' * (2 - len(x)) + x
      return x
  
  
  def solve():
      s = input("Please input the plaintext (ASCII): ")
      key_0 = [
          '66', '83', 'BE', 'E7', 'B3', '9C', '30', '98', '77', '17', '1A', '2A',
          '15', '70', 'A7', '94'
      ]
      l = len(s)
      cnt = 0
      state_num = l // 16
      if l % 16 != 0:
          print(
              "The len of plain text is not satisfy the mutiple of 16. So we pad as 00"
          )
      state_num += 1
      for k in range(state_num):
          array = [[0 for j in range(4)] for i in range(4)]
          for j in range(4):
              for i in range(4):
                  if cnt < l:
                      array[i][j] = ex0(hex(ord(s[cnt]))[2:])
                  else:
                      array[i][j] = '00'
                      if i == 3 and j == 3:
                          array[i][j] = ex0(hex(cnt - l)[2:])
                  cnt += 1
          print(f'state{k + 1} (16 Base): ')
          for i in range(4):
              for j in range(4):
                  print(array[i][j], end=" ")
              print()
  
          index = 0
          print(f'state{k + 1} after Round 0 (16 Base): ')
          for j in range(4):
              for i in range(4):
                  ans = int(array[i][j], 16) ^ int(key_0[index], 16)
                  array[i][j] = ex0(hex(ans)[2:])
                  index += 1
          for i in range(4):
              for j in range(4):
                  print(array[i][j], end=" ")
              print()
      return
  
  
  if __name__ == "__main__":
      solve()
  ```

  

* ### <font color=#000000 size=4 face=粗体>block的异或运算</font>

  输入为16进制的明文和16进制的key，明文长度必须是key的倍数

  ```python
  '''
  Author: NEFU AB-IN
  Date: 2021-12-09 20:06:58
  FilePath: \DCAS\blockXor.py
  LastEditTime: 2021-12-16 11:10:38
  '''
  
  
  def xor(x, k):
      x = int(x, 16)
      k = int(k, 16)
      return hex(x ^ k)[2:]
  
  
  def solve():
      plaintext = input("Please input plaintext (16 Base): ")
      key = input("Please input key (16 Base): ")
      multiple = len(plaintext) / len(key)
      if int(multiple) != multiple:
          print("len of string is not satisfied")
          return
      ans = ""
      index = 0
      multiple = int(multiple)
      for i in range(0, len(plaintext), len(key)):
          ans += xor(plaintext[index:index + len(key)], key)
          # 分key的段进行操作，并将16进制转成10进制进行异或，得出来的再进行转16进制
          # 从而变成字符串，去后两位即可
          index += len(key)
      print("ciphertext as hex (16 Base): " + ans)
  
  
  if __name__ == "__main__":
      solve()
  
  ```

  


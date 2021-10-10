Powered by:**NEFU AB_IN**

# <font color=#6495ED size=6>powershell Git脚本</font>

* 相比之下还是比较好写的，这里为了记一下

  记录两个输入的变量

  ```powershell
  param($a, $b)
  ```
	输入$a$变量，并与终端进行交互

  ```powershell
  $a = Read-Host 'Commit Info?'
  ```

  完整代码

  ```powershell
  param($a, $b)
  echo 'Wait a second, Turnning VPN on'
  cd C:\Users\liusy\Desktop\Daily\
  .\v2rayN.lnk
  echo 'Successful Turning'
  $a = Read-Host 'Commit Info?'
  cd d:\Code\Vscode
  git add .
  git commit -m "$a"
  git push origin master
  ```

  


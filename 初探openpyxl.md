Powered by:**NEFU AB_IN**

@[TOC](文章目录)

# <font color=#6495ED size=6>初探openpyxl</font>

* ## 介绍

  $openpyxl$是用于读取/写入$Excel 2010 xlsx/xlsm$文件的$Python$库，也就是说$openpyxl$这个$Python$库不支持$xls$文件的读取和操作

* ## lesson1_Load.py

  ```python
  '''
  Author: NEFU AB_IN
  Date: 2021-08-31 12:20:51
  FilePath: \Vscode\Excel&python\lesson1_Load.py
  LastEditTime: 2021-08-31 12:47:47
  '''
  from openpyxl import Workbook, load_workbook
  #适用于2010以上excel
  
  wb = load_workbook('test.xlsx') # 读取excel档案
  ws = wb.active # 选取工作表(这里选取的是默认的)
  print(ws)
  #<Worksheet "Sheet1">
  print(ws['A5'])
  #<Cell 'Sheet1'.A5>
  print(ws['A5'].value) #取得表格内容Cell
  #NEFU
  ws['C12'].value = '2019级' #修改
  #-------------------------------------------------
  print(wb.sheetnames)
  #['Sheet1', 'Sheet2', 'Sheet3']
  #-------------------------------------------------
  ws = wb['Sheet2'] #自定义选择工作表
  print(ws)
  #<Worksheet "Sheet2">
  #-------------------------------------------------
  wb.create_sheet('lsy') #创建工作表
  print(wb.sheetnames)
  #['Sheet1', 'Sheet2', 'Sheet3', 'lsy']
  #-------------------------------------------------
  
  
  wb.save('test.xlsx') #保存修改才会生效
  
  ```

* ## lesson2_Create

  ```python
  '''
  Author: NEFU AB_IN
  Date: 2021-08-31 12:46:39
  FilePath: \Vscode\Excel&python\lesson2_Create.py
  LastEditTime: 2021-08-31 12:53:05
  '''
  from openpyxl import Workbook, load_workbook
  
  wb = Workbook()
  ws = wb.active
  ws.title = 'lsy' #命名工作表
  ws['A1'].value = 1
  
  ws.append([123, 456, 789, 0]) #新增一横排的列表，不会覆盖
  ws.append([123, 456, 789, 0]) 
  ws.append([123, 456, 789, 0]) 
  
  wb.save('new_excel.xlsx') #如果存在就会覆盖
  ```

* ## lesson3_Load1

  ```python
  '''
  Author: NEFU AB_IN
  Date: 2021-08-31 12:54:20
  FilePath: \Vscode\Excel&python\lesson3_Load1.py
  LastEditTime: 2021-08-31 13:09:37
  '''
  from openpyxl import Workbook, load_workbook
  from openpyxl.utils import get_column_letter
  
  wb = load_workbook('new_excel.xlsx')
  ws = wb.active
  
  for row in range(1, 5): #必须以1作为起始
      for col in range(1, 5):
          col_char = get_column_letter(col) # 比如 1->A
          print(ws[col_char + str(row)].value, end = " ")
      print()
  wb.save('new_excel.xlsx')
  ```

* ## lesson4_Merge

  ```python
  '''
  Author: NEFU AB_IN
  Date: 2021-08-31 13:14:34
  FilePath: \Vscode\Excel&python\lesson4_Merge.py
  LastEditTime: 2021-08-31 13:15:59
  '''
  from openpyxl import Workbook, load_workbook
  
  wb = load_workbook('new_excel.xlsx')
  ws = wb.active
  ws.merge_cells('A1:E1') #合并单元格
  ws.unmerge_cells('A1:E1') #还原，但原本的资料不会被恢复
  
  wb.save('new_excel.xlsx')
  
  
  ```

* ## lesson5_Insert

  ```python
  '''
  Author: NEFU AB_IN
  Date: 2021-08-31 14:49:59
  FilePath: \Vscode\Excel&python\lesson5_Insert.py
  LastEditTime: 2021-08-31 14:53:41
  '''
  from openpyxl import Workbook, load_workbook
  
  wb = load_workbook('new_excel.xlsx')
  ws = wb.active
  
  ws.insert_rows(3) #在第三行插入一空行
  ws.insert_cols(3)
  
  ws.delete_rows(3)
  ws.delete_cols(3)
  
  
  wb.save('new_excel.xlsx')
  ```

* ## lesson6_move

  ```python
  '''
  Author: NEFU AB_IN
  Date: 2021-08-31 14:55:19
  FilePath: \Vscode\Excel&python\lesson6_move.py
  LastEditTime: 2021-08-31 14:56:38
  '''
  from openpyxl import Workbook, load_workbook
  
  wb = load_workbook('new_excel.xlsx')
  ws = wb.active
  
  ws.move_range('A3:E4', rows=2, cols=2)
  #往右和往下各两格
  
  wb.save('new_excel.xlsx')
  ```

* ## Example

  ```python
  '''
  Author: NEFU AB_IN
  Date: 2021-08-31 15:31:15
  FilePath: \Vscode\Excel&python\example.py
  LastEditTime: 2021-08-31 15:35:34
  '''
  from openpyxl import Workbook, load_workbook
  from openpyxl.utils import get_column_letter
  from openpyxl.styles import Font
  # https://openpyxl.readthedocs.io/en/stable/styles.html
  
  data = [
      {
          'name': 'A',
          'tall': 180,
          'age': 23,
          'weight': 74
      },
      {
          'name': 'B',
          'tall': 177,
          'age': 28,
          'weight': 90
      },
      {
          'name': 'C',
          'tall': 160,
          'age': 30,
          'weight': 60
      },
      {
          'name': 'D',
          'tall': 155,
          'age': 50,
          'weight': 50
      },
      {
          'name': 'E',
          'tall': 170,
          'age': 46,
          'weight': 99
      }
  ]
  
  wb = Workbook()
  ws = wb.active
  
  title = ['name', 'height', 'old', 'weight']
  ws.append(title)
  
  for person in data:
      ws.append(list(person.values()))
  
  for col in range(2,5):
      char = get_column_letter(col)
      ws[char + '7'] = f'=AVERAGE({char}2:{char}6)'# 直接输入公式
  
  for col in range(1,5):
      char = get_column_letter(col)
      ws[char + '1'].font = Font(bold=True, color="000000FF") #粗体
  
  wb.save('data.xlsx')
  
  ```

完结。
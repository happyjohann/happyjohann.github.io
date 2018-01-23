---
layout: post
title: Python Learning Note 1 - Excel(xls, xlsx) Read and Write, Deep Copy ...
category: Python
tags: Note
date: 2016-02-20
---

Excel file read and write, list deep copy, integer to string convert and no line change print.
<!--more-->

# Python Learn and Recap

## python deep copy
```python
    a = [0,1,2,3,4,5,6,7,8,9,10]
    b = a[:] #deep copying the list a and assigning it to b
    b = copy.deepcopy(a) #this is also a way of deep copy
    print id(a)
    20983280
    print id(b)
    12967208

    a[2] = 20
    print a
    [0, 1, 20, 3, 4, 5, 6, 7, 8, 9,10]
    print b
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9,10]
```

## python count unique values in list
```python
    len(set(new_words))
```

## Python print without line change
```python
    python 2.x, print
    >>> print x, 
    python 3.x print
    >>> print(x, end="")
```

## int and string convertion
```python
    str to int: int_value = int(str_value)
    int to str: str_value = str(int_value)

    int -> unicode: unicode(int_value)
    unicode -> int: int(unicode_value)
    str -> unicode: unicode(str_value)
    unicode -> str: str(unicode_value)
    int -> str: str(int_value)
    str -> int: int(str_value)
```

## xls Read
```python
    import xlrd
    book = xlrd.open_workbook("myfile.xls")
    print "The number of worksheets is", book.nsheets
    print "Worksheet name(s):", book.sheet_names()
    sh = book.sheet_by_index(0)
    print sh.name, sh.nrows, sh.ncols
    print "Cell D30 is", sh.cell_value(rowx=29, colx=3)
    for rx in range(sh.nrows):
        print sh.row(rx)
    # Refer to docs for more details.
    # Feedback on API is welcomed.
```

For more:

- [xlrd Package](https://github.com/python-excel/xlrd)

## xls Write
```python
    import xlwt
    from datetime import datetime
    style0 = xlwt.easyxf('font: name Times New Roman, color-index red, bold on',
        num_format_str='#,##0.00')
    style1 = xlwt.easyxf(num_format_str='D-MMM-YY')
    wb = xlwt.Workbook()
    ws = wb.add_sheet('A Test Sheet')
    ws.write(0, 0, 1234.56, style0)
    ws.write(1, 0, datetime.now(), style1)
    ws.write(2, 0, 1)
    ws.write(2, 1, 1)
    ws.write(2, 2, xlwt.Formula("A3+B3"))
    wb.save('example.xls')
```

For more:

- [xlwt Package](https://github.com/python-excel/xlwt)

## xlsx Read
```python
    from openpyxl import load_workbook
    wb = load_workbook('ExampleData.xlsx', guess_types=True, read_only=True)

    from openpyxl import Workbook
    print(wb.get_sheet_names())
    print(wb.sheetnames)

    for ws in wb:
        print(ws.title)
    
    ws = wb.worksheets[0]
    ws = wb['Train Schedule']
    ws = wb.get_sheet_by_name("Train Schedule")

    print(ws.min_row)
    print(ws.min_column)
    print(ws.max_row)
    print(ws.max_column)

    print(ws.calculate_dimension())

    print(ws['A1'])
    print(ws.cell('A1'))
    print(ws.cell(row = 1, column = 1))
    print(ws.cell(1, 1))
    print(ws['A1':'C2'])

    tuple(ws.iter_rows('A1:C2'))
    for row in ws.iter_rows('A1:C2'):
           for cell in row:
               print(cell.coordinate)
    
    print(ws.rows)
    print(ws.columns)

    print(ws.columns[0][0].value)

    ws['A1'] = datetime.datetime(2010, 7, 21)
    ws['A1'].number_format
    
    ws['B1'] = '3.14%'
    ws['B1'].value          # 0.031400000000000004
    ws['B1'].number_format  # '0%'
```




    


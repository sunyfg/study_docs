# Python 自动化办公

## 文件操作利器 - shutil

### 文件的复制

导入包和模块

```py
from shutil import copy
```

使用方法

```
copy(来源地址, 目标地址)
```

### 文件内容的复制

导入包和模块：

```py
from shutil import copyfile
```

使用方法：

```py
copyfile(来源文件, 目标文件)
```

### 文件的裁剪（移动、重命名）

导入包和模块：

```py
from shutil import move
```

使用方法：

```
move(来源地址, 目标地址)
```

### 文件的删除

导入包和模块：

```py
from os import remove
```

使用方法：

```
remove(目标文件地址)
```

### 文件的压缩

导入包和模块：

```py
from shutil import make_archive
```

使用方法：

```py
make_archive(压缩之后的文件名, 压缩后缀, 希望压缩的文件或目录)
```

返回值：生成的压缩包地址

### 文件的解压缩

导入包和模块：

```py
from shutil import unpack_archive
```

使用方法：

```
unpack_archive(要解压的文件, 解压后的路径)
```

### 文件夹的复制

导入包和模块：

```py
from shutil import copytree
```

使用方法：

```
copytree(来源目录，目标目录)
```

注：目标目录不能存在，否则会报错: `FileExistsError: [Errno 17] File exists: 'test1'`  

### 文件夹的删除

导入包和模块：

```py
from shutil import rmtree
```

使用方法：

```
rmtree(目标目录)
```

注：目标目录必须存在，否则会报错：`FileNotFoundError: [Errno 2] No such file or directory: 'test4'` 

### 文件夹的裁剪（移动、重命名）

导入包和模块：

```py
from shutil import move
```

使用方法：

```py
move('来源目录', '目标目录')
```

注：当目标目录不存在，并且和来源目录属于项目路径下，属于重命名

## 文件查找 glob

### 功能介绍

`glob` 是一个快速查找文件夹中内容的包，我们可以通过模糊查找的形式找到我们想要的内容

### glob 的使用

导入包和模块：

```py
from glob import glob
```

使用方法：

```py
glob('任意目录')

# 示例
result = glob(target + '/*')
result = glob(target + '/*.zip')
result = glob(target + '/file*')
```

返回内容：

指定路径下的内容列表，不存在的路径返回空列表

### 练习1：查找指定文件

已知条件：想查找的文件名已知道，但目录在哪里不知道

实现方法：利用 `glob` 从最上级开始查找，利用递归模式不断查找，直到找到文件为止

```py
# coding: utf-8

# 获取当前路径下所有内容
# 判断每个内容的类型（文件夹还是文件）
# 递归

import glob
import os

path = os.path.join(os.getcwd(), '*')
final_result = []


def search(path, target):
    result = glob.glob(path)

    for data in result:
        if os.path.isdir(data):
            _path = os.path.join(data, '*')
            search(_path, target)
        else:
            if target in data:
                final_result.append(data)
    return final_result


if __name__ == '__main__':
    result = search(path, target='test1')
    print(result)

```

### 练习2：查找含有指定内容的文件

已知条件：文件中含有某些关键字，但不知道文件名和所在路径

实现方法：利用 `glob` 从最上级开始查找，利用递归模式，如果是文件夹则进入继续查找，是文件则读取，判断是否包含内容，返回含有该内容的文件名以及所在路径

```py
# coding: utf-8

# 获取当前路径下所有内容
# 判断每个内容的类型（文件夹还是文件）
# 递归

import glob
import os

path = os.path.join(os.getcwd(), '*')
final_result = []


def search(path, target):
    result = glob.glob(path)

    for data in result:
        if os.path.isdir(data):
            _path = os.path.join(data, '*')
            search(_path, target)
        else:
            f = open(data, 'r')
            try:
                content = f.read()
                if target in content:
                    final_result.append(data)
            except:
                print('data read failed: %s' % data)
                continue
            finally:
                f.close()
    return final_result


if __name__ == '__main__':
    result = search(path, target='tom')
    print(result)

```

### 练习3：清理重复文件

实现方法：可以从指定路径（或最上层路径）开始读取，利用 `glob` 读取每个文件夹，读到文件，记录名称和大小，每次都检测是否读取过相同名称的文件，如果存在，判断大小是否相同，如果相同，我们认为是重复文件，则删除

```py
import glob
import os
import hashlib
# md5 格式，只要内容不变，生成的MD5也不变

# {'name': {'path1/name': 'content', 'path2/name': 'content'}}
data = {}


def clear(path):
    result = glob.glob(path)

    for _data in result:
        if os.path.isdir(_data):
            _path = os.path.join(_data, '*')
            clear(_path)
        else:
            name = os.path.split(_data)[-1]
            is_byte = False
            mode = 'r'
            if '.zip' in name:
                is_byte = True
                mode = 'rb'
            f = open(_data, mode)
            content = f.read()
            f.close()
            if is_byte:
                hash_content_obj = hashlib.md5(content)
            else:
                hash_content_obj = hashlib.md5(content.encode('utf-8'))

            hash_content = hash_content_obj.hexdigest()
            
            if name in data:
                sub_data = data[name]
                is_delete = False
                for k, v in sub_data.items():
                    if v == hash_content:
                        os.remove(_data)
                        print('%s will delete' % _data)
                        is_delete = True
                if not is_delete:
                    data[name][_data] = hash_content
            else:
                data[name] = {
                    _data: hash_content
                }

if __name__ == '__main__':
    path = os.path.join(os.getcwd(), '*')
    clear(path)
    for k, v in data.items():
        # print('%s, %s' % (k, v))
        for _k, _v in v.items():
            print(_k, _v)

```

### 练习4：批量修改目录中的文件名称

已知条件：知道文件名需要修改的字符串

实现方法：通过循环，将目标字符串加入到文件名并进行修改

```py
# coding: utf-8

import glob
import os
import shutil

def update_name(path):
    result = glob.glob(path)

    for index, data in enumerate(result): # enumerate枚举
        if os.path.isdir(data):
            _path = os.path.join(data, '*')
            update_name(_path)
        else:
            path_list = os.path.split(data)
            # /home/xxx/name.txt => [/home/xxx, name.txt]
            name = path_list[-1]
            new_name = '%s_%s' % (index, name)  # 0_name.txt
            new_data = os.path.join(path_list[0], new_name)
            shutil.move(data, new_data)

if __name__ == '__main__':
    path = os.path.join(os.getcwd(), '*')
    update_name(path)

```

## Word 操作利器之 `python-docx` 

### python-docx 安装

```
pip install python-docx
```

### python-docx 之 Document

导入包和模块：

```py
from docx import Document
```

使用方法：

```py
Document('word地址')
```

返回值：word 文件对象

### 段落内容读取

```py
document_obj.paragraphs
```

使用方法：通过循环获取每个段落对象，并调用 `text` 

### 表格内容读取

```
document_obj.tables
```

使用方法：通过循环获取行与列的内容

### 案例

```py
from docx import Document

doc = Document('文本.docx')

# 段落读取
for p in doc.paragraphs:
    print(p.text)

# 表格读取
for t in doc.tables:
    for row in t.rows:
        _row_str = ''
        for cell in row.cells:
            # print(cell.text)
            _row_str += cell.text + ','
        print(_row_str)

```

### 练习：简历筛选

已知条件：想要查找包含指定关键字的简历

开发思路：批量读取每一个 word（通过glob获取word），将他们的所有可读内容获取，并通过关键字方式筛选，拿到目标简历地址

```py
# coding: utf-8

from docx import Document
import glob
import os

class ReadDoc(object):
    def __init__(self, path):
        self.doc = Document(path)
        self.p_text = ''
        self.table_text = ''

        self.get_para()
        self.get_table()

    def get_para(self):
        for p in self.doc.paragraphs:
            self.p_text += p.text + '\n'

    def get_table(self):
        for t in self.doc.tables:
            for row in t.rows:
                _cell_str = ''
                for cell in row.cells:
                    _cell_str += cell.text + ','
                self.table_text += _cell_str + '\n'

def search_word(path, targets):
    result = glob.glob(path)
    final_result = []

    for i in result:
        if os.path.isfile(i):
            if i.endswith('.docx'):
                is_use = True
                doc = ReadDoc(i)
                p_text = doc.p_text
                t_text = doc.table_text
                all_text = p_text + t_text

                for target in targets:
                    if target not in all_text:
                        is_use = False
                        break
                if not is_use:
                    continue
                final_result.append(i)
    return final_result

if __name__ == '__main__':
    path = os.path.join(os.getcwd(), '*')
    res = search_word(path, ['python', 'golang', 'react'])
    print(res)

```

## 生成 word 文档

### 生成标题

使用方法：

```py
titleObj = DocumentObj.add_heading('标题内容', '标题样式级别')
```

标题样式级别：0 <= level <= 9

内容追加：

```py
titleObj.add_run('字符串内容')
```

### 添加段落

使用方法：

```py
para_obj = document_obj.add_paragraph('段落内容')
```

内容追加：

```py
para_obj.add_run('字符串内容')
```

换行方式：使用 `\n` 换行特殊字符来分割段落

### 添加图片

使用方法：

```py
image_obj = document_obj.add_picture('图片地址', '宽', '高')
```

宽高定义：

```py
from docx.shared import Inches # 像素化函数
add_picture(x, width=Inches(5), height=Inches(5))
```

### 添加表格

使用方法：

```py
table_obj = document_obj.add_table(rows='行数', cols='列数')
cells = table_obj.rows[0].cells
cells[0].text = '当前行0列的内容'
cells[1].text = '当前行1列的内容'
```

表格追加：

```py
row_cell = table.add_row().cells
```

### 分页

使用方法：

```py
document_obj.add_page_break()
```

### 保存生成 word

使用方法：

```py
document_obj.save('文件地址') # => /home/test.docx
```

### 全局样式定义

使用方法：

```py
style = document_obj.styles['Normal']
for style in document_obj.styles:
	print(style)
```

字体：

```py
style.font.name = '微软雅黑'
```

字体颜色：

```py
from docx.shared import RGBColor

style.font.color.rgb = RGBColor(255, 0, 0)
```

字体大小：

```py
from docx.shared import Pt

style.font.size = Pt(20)
```

### 文本样式

字体：

```py
obj.font.name = '微软雅黑'
```

字体颜色：

```py
obj.font.color.rgb = RGBColor(255, 0, 0)
```

字体大小：

```py
obj.font.size = Pt(20)
```

标题居中：

```PY
from docx.enum.text import WD_PARAGRAPH_ALIGNMENT
obj.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER
```

字体斜体：

```py
obj.italic = True
```

字体加粗：

```py
obj.blod = True
```

### 图片居中

```py
from docx.enum.text import WD_ALIGN_PARAGRAPH

p = doc.add_paragraph()
p.alignment = WD_ALIGN_PARAGRAPH.CENTER
_p = p.add_run()
_p.add_picture('logo.png')
```

### 表格样式

获取表格样式类型

获取表格样式

```py
from docx.enum.style import WD_STYLE_TYPE
```

定义表格：

```py
document_obj.add_table(rows, cols, style)
```



## 生成 PDF

### pdf 工具包 - pdfkit

```sh
pip install pdfkit
```

依赖安装 wkhtmltopdf 包，下载地址：[https://wkhtmltopdf.org/downloads.html](https://wkhtmltopdf.org/downloads.html)

### html 转 pdf

```py
pdfkit.from_file('html文件', '保存路径')
```

### 网址 转 pdf

```py
pdfkit.from_url('网址', '保存路径')
```

### 字符串 转 pdf

```py
pdfkit.from_string('基于 html 的字符串', '保存路径')
```

### word 转 pdf

```sh
pip install pydocx
```

```py
from pydocx import PyDocX
html = PyDocX.to_html(word.docx)
f = open(...)
```

异常处理：

```py
# 异常信息
AttributeError: module 'collections' has no attribute 'Hashable'
```

解决方法：

```py
# 找到 pydocx/pydocx/util/memoize.py 文件
# 修改 if not isinstance(args, collections.Hashable): 
# 为 if not isinstance(args, collections.abc.Hashable):
```



## Excel 读取之 xlrd

### xlrd 安装

```
pip install xlrd

pip install xlrd==1.2.0
```

### 获取 excel 对象

```py
import xlrd

book = xlrd.open_workbook('excel 文件')
```

### 获取工作薄

| 函数名                | 说明               |
| --------------------- | ------------------ |
| book.sheet_by_name()  | 按照工作簿名称获取 |
| book.sheet_by_index() | 按照索引获取       |
| book.sheets()         | 获取所有工作簿列表 |

### 读取工作簿内容

| 函数名           | 说明             |
| ---------------- | ---------------- |
| sheet.nrows      | 返回总行数       |
| sheet.ncols      | 返回总列数       |
| sheet.get_rows() | 返回每行内容列表 |

### 示例

```py
# coding: utf-8

import xlrd

excel = xlrd.open_workbook('study.xlsx')
print(excel)

book = excel.sheet_by_name('学生手册')
print(book)

book = excel.sheet_by_index(0)
print(book.name)

for i in excel.sheets():
    print(i.name)

print(book.nrows)
print(book.ncols)

for i in book.get_rows():
    content = []
    for j in i:
        content.append(j.value)
    print(content)

```

## Excel 写入之 xlsxwriter

### xlsxwriter 安装

```sh
pip install xlsxwriter
```

### 初始化 excel 对象

```py
book = xlsxwriter.Workbook() # 生成 excel 对象
sheet = book.add_sheet('工作簿名称')
```

### 获取工作簿

| 函数名                | 说明            | 参数                 |
| --------------------- | --------------- | -------------------- |
| xlsxwriter.Workbook() | 生成 excel 对象 | Excel 文件名         |
| add_worksheet()       | 添加工作簿      | 工作簿名称           |
| sheet.write()         | 书写内容        | 行索引、列索引、内容 |
| book.close()          | 关闭 excel 对象 | 无                   |

### 生成图表

| 函数名       | 说明               | 参数         |
| ------------ | ------------------ | ------------ |
| add_chart()  | 创建图表对象       | {type: 样式} |
| add_series() | 定义需要展示的数据 | 字典         |
| set_title()  | 定义图表title      | 字符创       |

### add_series 参数

| 参数       | 说明       | 值                |
| ---------- | ---------- | ----------------- |
| categories | 展示的标题 | =Sheet1!$A$1:$A$4 |
| values     | 展示的数据 | =Sheet1!$B$1:$B$4 |
| name       | 表名       |                   |

### 图表样式

| 样式名   | 说明       |
| -------- | ---------- |
| area     | 区域样式表 |
| bar      | 条形样式   |
| column   | 柱状样式   |
| line     | 线条样式   |
| pie      | 饼图样式   |
| doughnut | 圆环样式   |
| scatter  | 散点样式   |
| stock    | 库存样式   |
| radar    | 雷达样式   |

### 示例

```py
# coding: utf-8

import xlsxwriter
import xlrd

# excel = xlsxwriter.Workbook('write.xlsx')
# sheet = excel.add_worksheet('study')
#
# title = ['姓名', '性别', '年龄', '成绩', '等级']
#
# for index, data in enumerate(title):
#     sheet.write(0, index, data)
#
# excel.close()

def read():
    result = []
    excel = xlrd.open_workbook('study.xlsx')
    book = excel.sheet_by_name('学生手册')
    for i in book.get_rows():
        content = []
        for j in i:
            content.append(j.value)
        result.append(content)
    return result

def write(content):
    excel = xlsxwriter.Workbook('write.xlsx')
    book = excel.add_worksheet('study')

    for index, data in enumerate(content):
        print(data)
        for sub_index, sub_data in enumerate(data):
            book.write(index, sub_index, sub_data)

    book1 = excel.add_worksheet('学生等级')
    data = [
        ['优秀', '良好', '中', '差'],
        [1100, 2000, 1000, 900]
    ]
    book1.write_column('A1', data[0])
    book1.write_column('B1', data[1])
    chart = excel.add_chart({'type': 'column'})
    chart.add_series({
        'categories': '=学生等级!$A1:$A4',
        'values': '=学生等级!$B1:$B4',
        'name': '成绩占比'
    })
    chart.set_title({'name': '成绩占比图表'})
    book1.insert_chart('A10', chart)

    excel.close()


if __name__ == '__main__':
    result = read()
    write(result)

```



## PPT 操作之 python-pptx

### 安装

```
pip install python-pptx

import pptx
```

### 创建ppt对象

```py
# coding: utf-8

import pptx

p = pptx.Presentation()  # 生成一个ppt对象

layout = p.slide_layouts[7]  # 选择布局
# 0 title subtitle
# 1 title content 布局

slide = p.slides.add_slide(layout)

p.save('test1.ppt')

```

### 获取段落

```py
# coding: utf-8

import pptx
p = pptx.Presentation()
layout = p.slide_layouts[1]
slide = p.slides.add_slide(layout)
placeholder = slide.placeholder[1] # 获取段落
p.save('test2.ppt')
```

### 段落中添加数据

```py
placeholder.text = 'content'
```

### 段落中定义多个段落

```
paragraph = placeholder.text_frame.add_paragraph()
```

| 函数名           | 说明       |
| ---------------- | ---------- |
| `text`           | 定义内容   |
| `font.bold`      | 文字加粗   |
| `font.italic`    | 文字斜体   |
| `font.size`      | 字体大小   |
| `alignment`      | 段落位置   |
| `color.rgb`      | 字体颜色   |
| `font.underline` | 文字下划线 |

### 插入表格

```py
p = pptx.Presentation()
layout = p.slide_layouts[1]
slide = p.slides.add_slide(layout)
table = slide.shapes.add_table('行数', '列数', 'left', 'top', 'width', 'height').table
table.cell('行', '列').text = 'content'
```

### 插入图片

```py
p = pptx.Presentation()
layout = p.slide_layouts[1]
slide = p.slides.add_slide(layout)
image = slide.shapes.add_picture('image_file', 'left', 'top', 'width', 'height')
```

### 示例

```py
# coding: utf-8

import pptx
from pptx.util import Pt, Inches
from pptx.enum.text import PP_PARAGRAPH_ALIGNMENT
from pptx.dml.color import RGBColor

p = pptx.Presentation()

layout = p.slide_layouts[1]

slide = p.slides.add_slide(layout)

placeholder = slide.placeholders[1]  # 0 title 1 content
# placeholder.text = '欢迎学习ppt制作\n欢迎学习Python'

title = slide.placeholders[0]
title.text = '题目'

# 多个段落
paragraph1 = placeholder.text_frame.add_paragraph()
paragraph1.text = '欢迎学习PPT制作'
paragraph1.font.bold = True
paragraph1.font.italic = True
paragraph1.font.size = Pt(16)
paragraph1.font.underline = True
paragraph1.alignment = PP_PARAGRAPH_ALIGNMENT.CENTER

paragraph2 = placeholder.text_frame.add_paragraph()
paragraph2.text = '欢迎学习Python'
paragraph2.font.size = Pt(32)
paragraph2.alignment = PP_PARAGRAPH_ALIGNMENT.RIGHT

# 第二页
layout = p.slide_layouts[6]
slide = p.slides.add_slide(layout)
left = top = width = height = Inches(5)
box = slide.shapes.add_textbox(left, top, width, height)
para = box.text_frame.add_paragraph()

para.text = 'this is a para test'
para.alignment = PP_PARAGRAPH_ALIGNMENT.CENTER
para.font.size = Pt(32)
para.font.color.rgb = RGBColor(255, 255, 0)
para.font.name = '微软雅黑'

# 第三页，表格
layout = p.slide_layouts[1]
slide = p.slides.add_slide(layout)

rows = 10
cols = 2

left = top = Inches(2)
width = Inches(6.0)
height = Inches(1.0)

table = slide.shapes.add_table(rows, cols, left, top, width, height).table

for index, _ in enumerate(range(rows)):
    for sub_index in range(cols):
        table.cell(index, sub_index).text = '%s:%s' % (index, sub_index)

layout = p.slide_layouts[6]
slide = p.slides.add_slide(layout)

image = slide.shapes.add_picture(
    image_file='logo2020.png',
    left=Inches(1),
    top=Inches(1),
    width=Inches(6),
    height=Inches(4)
)

p.save('test2.ppt')

```

### 读取 PPT

获取整体ppt对象

```
p = Presentation(ppt文件)

p.slides  # 返回一个幻灯列表，获取所有 ppt 内容

slide.shapes  # 返回具体 ppt 中的形状
```

获取文本内容

```
shape.has_text_frame  # 判断是否是文本内容

shape.text_frame.text  # 获取文本内容
```

获取表格内容

```
shape.has_table   # 判断是否是表格类型

shape.table.iter_cells()  # 获取文本内容列表
```

示例：

```py
# coding: utf-8

import pptx

p = pptx.Presentation('test2.ppt')

for slide in p.slides:
    for shape in slide.shapes:
        if shape.has_text_frame:
            print(shape.text_frame.text)
        if shape.has_table:
            for cell in shape.table.iter_cells():
                print(cell.text)

```



## 发送邮件

### 发送邮件流程

- 登录邮箱
- 书写接收者邮箱
- 书写题目与内容（添加附件）
- 点击发送

### 邮件协议

- `smtp` 是邮件发送的协议
- `pop3` 是邮件接收协议

协议是一种规则，已经被底层网络封装好，我么无需关系他的具体规则是什么，直接使用上层工具即可。

### smtplib 包

创建协议对象

```
smtpObj = smtplib.SMTP()
```

创建链接

```
smtpObj.connect('smtp服务器地址', 25)
```

登录验证

```
smtpObj.login(mail_user, mail_pass)
```

发送邮件

```py
smtpObj.sendmail(sender, receivers, message) # message 是消息对象加密字符串
```

### email 包 

| 函数名   | 参数                         | 说明                             |
| -------- | ---------------------------- | -------------------------------- |
| MIMEText | 邮件内容、邮件类型、编码格式 | 定义邮件发送内容的对象           |
| Header   | 各类信息、编码格式           | 将各类信息定义成对象，比如标题等 |

```py
message['From'] = Header(sender)  # 发送者
title = 'Python SMTP 邮件测试 content2'  # title
message['Subject'] = Header(title, 'utf-8')
```

### 发送邮件一些避坑总结

- 发送者的邮箱开通 `smtp` 与 `pop3` 的访问许可
- 或许密码要使用授权码
- 开通授权有些邮件可能会收费

### 邮件类型

| 邮件类型 | 说明         |
| -------- | ------------ |
| `plain`  | 普通文件类型 |
| `html`   | Html 类型    |

```py
message = MIMEText('<p style="color: red;">这是一个测试</p>', 'html', 'utf-8')
```

### 附件

| 函数名          | 参数 | 说明                 |
| --------------- | ---- | -------------------- |
| `MIMEMultipart` | 无   | 定义带附件的邮件对象 |

```py
from email.mime.multipart import MIMEMultipart
```

### 示例

```py
# coding: utf-8

import smtplib

from email.mime.text import MIMEText
from email.header import Header
from email.mime.multipart import MIMEMultipart


# 第三方 smtp
# 找到 smtp 开通，是否存在授权码
mail_host = 'smtp.163.com'
mail_user = '15021815774@163.com'
mail_pass = 'JDYVPQMODORHBRWO'

sender = '15021815774@163.com'
receivers = ['yanfeng_sun@yeah.net']

# message = MIMEText('这是一个测试', 'plain', 'utf-8')
# message = MIMEText('<p style="color: red;">这是一个测试</p>', 'html', 'utf-8')

# 发送附件
message = MIMEMultipart()

message['From'] = Header(sender)
message['Subject'] = Header('python 脚本测试', 'utf-8')
# print(message.as_string())

attr = MIMEText(open('send.py', 'r').read(), 'base64', 'utf-8')
attr['Content-Type'] = 'application/octet-stream'
attr['Content-Disposition'] = 'attachment;filename="send.py"'

message.attach(attr)
message.attach(MIMEText('这是一个带附件的邮件', 'plain', 'utf-8'))

try:
    smtpObj = smtplib.SMTP()
    smtpObj.connect(mail_host, 25)
    smtpObj.login(mail_user, mail_pass)
    smtpObj.sendmail(sender, receivers, message.as_string())
except smtplib.SMTPException as e:
    print('error: %s' % e)

```



## 定时模块使用

### Schedule 模块介绍

定时任务：在特定的时间自动执行一些任务的功能，python 中的 schedule 模块可以使我们方便简单的使用定时任务。

```
pip install schedule
```

### Schedule 各种时间用法

每过多少分钟执行一次 func 函数，args 是函数的参数

```py
schedule.every(count).minutes.do(func, args)
```

每天的 10 点 20 分执行一次 func 函数，args 是函数参数

```py
schedule.every().day.at('10:20').do(func, args)
```

### Schedule 支持的时间

| 类型      | 说明 |
| --------- | ---- |
| `minutes` | 分钟 |
| `seconds` | 秒   |
| `day`     | 天   |
| `hour`    | 小时 |
| `week`    | 周   |

### Schedule 的启动

```py
schedule.run_pending()
```

放在 `while` 中执行，并利用时间进行至少 1 秒阻塞。

### 示例：定时发送邮件

```py
# coding: utf-8

import smtplib

from email.mime.text import MIMEText
from email.header import Header
from email.mime.multipart import MIMEMultipart

import schedule
import time


# 第三方 smtp
# 找到 smtp 开通，是否存在授权码
mail_host = 'smtp.163.com'
mail_user = '15021815774@163.com'
mail_pass = 'JDYVPQMODORHBRWO'

sender = '15021815774@163.com'
receivers = ['yanfeng_sun@yeah.net']

# message = MIMEText('这是一个测试', 'plain', 'utf-8')
# message = MIMEText('<p style="color: red;">这是一个测试</p>', 'html', 'utf-8')

# 发送附件
message = MIMEMultipart()

message['From'] = Header(sender)
message['Subject'] = Header('python 脚本测试', 'utf-8')
# print(message.as_string())

attr = MIMEText(open('send.py', 'r').read(), 'base64', 'utf-8')
attr['Content-Type'] = 'application/octet-stream'
attr['Content-Disposition'] = 'attachment;filename="send.py"'

message.attach(attr)
message.attach(MIMEText('这是一个带附件的邮件', 'plain', 'utf-8'))

def send():
    print('send start')
    try:
        smtpObj = smtplib.SMTP()
        smtpObj.connect(mail_host, 25)
        smtpObj.login(mail_user, mail_pass)
        smtpObj.sendmail(sender, receivers, message.as_string())
    except smtplib.SMTPException as e:
        print('error: %s' % e)


if __name__ == '__main__':
    schedule.every(10).seconds.do(send)

    while 1:
        schedule.run_pending()
        time.sleep(1)

```
































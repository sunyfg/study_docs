## 更新 pip

```
python -m pip install --upgrade pip
```

### 查看 pip 版本

```
pip --version
```

### pip 国内镜像

```
清华：https://pypi.tuna.tsinghua.edu.cn/simple

中国科学技术大学 : https://pypi.mirrors.ustc.edu.cn/simple

豆瓣：http://pypi.douban.com/simple/

阿里云：http://mirrors.aliyun.com/pypi/simple/

安装
pip install --trusted-host pypi.douban.com ipython

删除
pip uninstall ipython

easy_install django
```

## Colorama 模块

Python 程序向控制台输出彩色文字

```sh
pip install colorama

# or

pip install colorama -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### 向控制台输出颜色

```python
from colorama import Back, Fore, Style

print(Fore.LIGHTBLUE_EX, "Hello World")  # 字体颜色
print(Back.LIGHTGREEN_EX, "Hello World") # 背景颜色
print(Style.RESET_ALL, "Hello World")    # 重置
```

## mysql-connector-python 模块

操作 mysql 数据库

```
pip install mysql-connector-python
```



## Python 线程池

### 为什么引入线程池？

如果程序中经常用到线程，频繁的创建和销毁线程会浪费很多硬件资源，所以需要把线程和任务分离开。线程可以反复利用，省去了重复创建的麻烦。

### 使用

```py
from concurrent.futures import ThreadPoolExecutor

def say_hello():
	print('Hello')

executor = ThreadPoolExecutor(50)
for i in range(0,10):
	executor.sumit(say_hello)
```




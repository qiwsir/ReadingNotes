# Python导入模块的几种姿势

https://codingpy.com/article/python-import-101/

1. 常规导入

   ```python
   import sys
   import os, sys, time    # 导入多个模块
   import sys as sys system    # 导入模块更名
   import urllib.error    # 直接导入模块中的某个子模块、或函数
   ```

2. from导入

   ```python
   from requests import get   # 从模块中导入具体的子模块或者函数
   from os import *    # 虽然省事，但是容易扰乱命名空间
   ```

3. 相对导入

   ```python
   my_package/
       __init__.py
       subpackage1/
           __init__.py
           module_x.py
           module_y.py
       subpackage2/
           __init__.py
           module_z.py
       module_a.py
       
       # 在module_x.py中：
       from .module_y import spam as ham    # spam是moduly_y.py中的函数，导入并更名
   ```

4. 可选导入

   ```python
   try:
       # For Python 3
       from http.client import responses
   except ImportError:  # For Python 2.5-2.7
       try:
           from httplib import responses  # NOQA
       except ImportError:  # For Python 2.4
           from BaseHTTPServer import BaseHTTPRequestHandler as _BHRH
           responses = dict([(k, v[0]) for k, v in _BHRH.responses.items()])
   ```

5. 局部导入

   ```
   import sys  # global scope
   
   def square_root(a):
       # This import is into the square_root functions local scope
       import math
       return math.sqrt(a)
   
   def my_pow(base_num, power):
       return math.pow(base_num, power)    # 这里会报错，因为import math在square_root作用域
   
   if __name__ == '__main__':
       print(square_root(49))
       print(my_pow(2, 3))
   ```

   
---
layout:     post
title:      BeautifulSoup4
subtitle:   BeautifulSoup4-一个对网页操作的库
date:       2023-04-20
author:     金皮皮
header-img: img/v2-4bed12d53f95797b389478104565a5fe_r.jpg
catalog:   true
tags:
    - python
---
# BeautifulSoup4

## 一、构建文档树

| 文档树对象       | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| Tag              | 标签; 访问方式:soup.tag;属性:tag.name(标签名)，tag.attrs(标签属性) |
| Navigable String | 可遍历字符串; 访问方式:soup.tag.string                       |
| BeautifulSoup    | 文档全部内容，可作为Tag对象看待; 属性:soup.name(标签名)，soup.attrs(标签属性) |
| Comment          | 标签内字符串的注释; 访问方式:soup.tag.string                 |

```
import lxml
import requests
from bs4 import BeautifulSoup

html =  """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!--Elsie--></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

#1、BeautifulSoup对象
soup = BeautifulSoup(html,'lxml')
print(type(soup))

#2、Tag对象
print(soup.head,'\n')
print(soup.head.name,'\n')
print(soup.head.attrs,'\n')
print(type(soup.head))

#3、Navigable String对象
print(soup.title.string,'\n')
print(type(soup.title.string))

#4、Comment对象
print(soup.a.string,'\n')
print(type(soup.a.string))

#5、结构化输出soup对象
print(soup.prettify())
```

## 二、遍历文档树

```
import requests
import lxml
import json
from bs4 import BeautifulSoup

html =  """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!--Elsie--></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html,'html.parser')

#1、向下遍历
print(soup.p.contents)
print(list(soup.p.children))
print(list(soup.p.descendants))

#2、向上遍历
print(soup.p.parent.name,'\n')
for i in soup.p.parents:
    print(i.name)

#3、平行遍历
print('a_next:',soup.a.next_sibling)
for i in soup.a.next_siblings:
    print('a_nexts:',i)
print('a_previous:',soup.a.previous_sibling)
for i in soup.a.previous_siblings:
    print('a_previouss:',i)
```

## 三、搜索文档树

```
import requests
import lxml
import json
from bs4 import BeautifulSoup

html =  """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!--Elsie--></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html,'html.parser')

#1、find_all( )
print(soup.find_all('a'))  #检索标签名
print(soup.find_all('a',id='link1')) #检索属性值
print(soup.find_all('a',class_='sister')) 
print(soup.find_all(text=['Elsie','Lacie']))

#2、find( )
print(soup.find('a'))
print(soup.find(id='link2'))

#3 、向上检索
print(soup.p.find_parent().name)
for i in soup.title.find_parents():
    print(i.name)
    
#4、平行检索
print(soup.head.find_next_sibling().name)
for i in soup.head.find_next_siblings():
    print(i.name)
print(soup.title.find_previous_sibling())
for i in soup.title.find_previous_siblings():
    print(i.name)
```

## 四、CSS选择器

```
import requests
import lxml
import json
from bs4 import BeautifulSoup

html =  """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!--Elsie--></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html,'html.parser')

print('标签查找:',soup.select('a'))
print('属性查找:',soup.select('a[id="link1"]'))
print('类名查找:',soup.select('.sister'))
print('id查找:',soup.select('#link1'))
print('组合查找:',soup.select('p #link1'))
```

## 五、文本替换

```
from bs4 import BeautifulSoup

with open("/var/www/html/Test/index.html", "r") as f:
 soup = BeautifulSoup(f, "lxml")

f = open("/var/www/html/Test/I18N_index.html", "w+")

for h2 in soup.find_all('h2'):
    i18n_string = "I18N_"+h2.string
    h2.string.replace_with(i18n_string)
    print(h2.string)

f.write(str(soup))
```


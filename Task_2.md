# 一. Beautiful Soup库入门
## Beautiful Soup库入门
1. Beautiful Soup是一个HTML/XML的解析器
2. 他是基于HTML DOM，会载入整个文档，解析整个DOM树，因此时间和内存开销都会比较大，性能要低于lxml
3. 匹配效率远不如正则以及xpath，一般不推荐正则的使用

## Beautiful Soup库的基本元素
1. Beautiful Soup库的理解：解析、遍历、维护“标签树”的功能库，对应一个HTML/XML的全部内容
2. BeautiluSoup的基本元素;
    - Tag标签：最基本的信息组指单元，分别用<>和</>标明开头和结尾
    - Name标签的名字，<p>...</p>的名字是'p'，格式：<tag>.name
    - Attribute标签的属性，字典形式组织，格式：<tag>.attrs
    - NavigableString标签内非属性字符串，<>...</>中字符串，格式：<tag>.string。
    - Comment标签字符串的注释部分，一种特殊的Comment类型

## 实例

```
from bs4 import BeautifulSoup
import requests
r = requests.get(https://python123.io/ws/demo.html)
demo = r.text
soup = BeautifulSoup(demo, "html.parser")
#print(soup.prettify())
print(soup.a)  #访问标签a
print(soup.a.parent.name) #访问标签的名字
print(soup.a.attrs) #访问标签的属性
print(soup.a.string) #标签内非属性字符串

```
- bs4库将任何HTML输入变成utf-8编码
- Python 3.x默认支持编码是utf-8，解析无障碍

## 基于bs4库的HTML内容遍历方法
- 标签树的下行遍历
    - .contents 子节点的列表，将<tag>所有儿子节点存入列表
    - .children 子节点的迭代类型，与.contents类似，用于循环遍历儿子节点
    - .descendants 子孙节点的迭代类型，包含所有子孙节点，用于循环遍历
- 标签树的上行遍
    - .parent 节点的父亲标签
    - .parents 节点先辈标签的迭代类型，用于循环遍历先辈节点
- 标签树的平行遍历
    - .next_sibling 返回按照HTML文本顺序的下一个平行节点标签
    - .previous_sibling 返回按照HTML文本顺序的上一个平行节点标签
    - .next_siblings 迭代类型，返回按照HTML文本顺序的后续所有平行节点标签
    - .previous_siblings 迭代类型，返回按照HTML文本顺序的前续所有平行节点标签

## 基于bs4库的HTML内容的查找方法
- <>.find_all(name, attrs, recursive, string, **kwargs)
    - 参数：
    - ∙ name : 对标签名称的检索字符串
    - ∙ attrs: 对标签属性值的检索字符串，可标注属性检索
    - ∙ recursive: 是否对子孙全部检索，默认True
    - ∙ string: <>…</>中字符串区域的检索字符串
        - 简写：
        - <tag>(..) 等价于 <tag>.find_all(..)
        - soup(..) 等价于 soup.find_all(..)
- 扩展方法
    - <>.find() 搜索且只返回一个结果，同.find_all()参数
    - <>.find_parents() 在先辈节点中搜索，返回列表类型，同.find_all()参数
    - <>.find_parent() 在先辈节点中返回一个结果，同.find()参数
    - <>.find_next_siblings() 在后续平行节点中搜索，返回列表类型，同.find_all()参数
    - <>.find_next_sibling() 在后续平行节点中返回一个结果，同.find()参数
    - <>.find_previous_siblings() 在前序平行节点中搜索，返回列表类型，同.find_all()参数
    - <>.find_previous_sibling() 在前序平行节点中返回一个结果，同.find()参数

# xpath
## Xpath常用的路劲表达式
- Xpath即为XML路劲语言，他是一种确定XML文档中部分位置的语言
- 在Xpath中，有7种类型的节点：元素、属性、文本、命名空间、处理指令、注释以及文档（根）节点
- XML文档是被作为处理节点树来对待的

Xpath使用路劲表达式在XML文档钟选取节点。节点是通过沿着路劲选取的，下面列出了最常用的路劲表达式：
- nodename 选取此节点的所有子节点。
- / 从根节点选取。
- // 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。
- . 选取当前节点。
- .. 选取当前节点的父节点。
- @ 选取属性。
- /text() 提取标签下面的文本内容
    - 如：
    - /标签名 逐层提取
    - /标签名 提取所有名为<>的标签
    - //标签名[@属性=“属性值”] - 提取包含属性为属性值的标签
    - @属性名 代表取某个属性名的属性值

##使用lxml解析
- 导入库：from lxml import etree

- lxml将html文本转成xml对象

    - tree = etree.HTML(html)
- 用户名称：tree.xpath(’//div[@class=“auth”]/a/text()’)

- 回复内容：tree.xpath(’//td[@class=“postbody”]’) 因为回复内容中有换行等标签，所以需要用string()来获取数据。

    - string()的详细见链接：https://www.cnblogs.com/CYHISTW/p/12312570.html
- Xpath中text()，string()，data()的区别如下：

    - text()仅仅返回所指元素的文本内容。
    - string()函数会得到所指元素的所有节点文本内容，这些文本讲会被拼接成一个字符串。
    - data()大多数时候，data()函数和string()函数通用，而且不建议经常使用data()函数，有数据表明，该函数会影响XPath的性能。
    
# 正则表达式
- 测试字符串内的模式。
    例如，可以测试输入字符串，以查看字符串内是否出现电话号码模式或信用卡号码模式。这称为数据验证。
- 替换文本。
    可以使用正则表达式来识别文档中的特定文本，完全删除该文本或者用其他文本替换它。
- 基于模式匹配从字符串中提取子字符串。
    可以查找文档内或输入域内特定的文本。

## 正则表达式语法
- . 表示任何单个字符
-[ ] 字符集，对单个字符给出取值范围 ，如[abc]表示a、b、c，[a‐z]表示a到z单个字符
- [^ ] 非字符集，对单个字符给出排除范围 ，如[^abc]表示非a或b或c的单个字符
- * 前一个字符0次或无限次扩展，如abc 表示 ab、abc、abcc、abccc等
- + 前一个字符1次或无限次扩展 ，如abc+ 表示 abc、abcc、abccc等
- ? 前一个字符0次或1次扩展 ，如abc? 表示 ab、abc
- | 左右表达式任意一个 ，如abc|def 表示 abc、def

- {m} 扩展前一个字符m次 ，如ab{2}c表示abbc

- {m,n} 扩展前一个字符m至n次（含n） ，如ab{1,2}c表示abc、abbc
- ^ 匹配字符串开头 ，如^abc表示abc且在一个字符串的开头
- $ 匹配字符串结尾 ，如abc$表示abc且在一个字符串的结尾
- ( ) 分组标记，内部只能使用 | 操作符 ，如(abc)表示abc，(abc|def)表示abc、def
- \d 数字，等价于[0‐9]
- \w 单词字符，等价于[A‐Za‐z0‐9_]


## re库的主要功能
- re.search() 在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象
    re.search(pattern, string, flags=0)
- re.match() 从一个字符串的开始位置起匹配正则表达式，返回match对象
    re.match(pattern, string, flags=0)
- re.findall() 搜索字符串，以列表类型返回全部能匹配的子串
    re.findall(pattern, string, flags=0)
- re.split() 将一个字符串按照正则表达式匹配结果进行分割，返回列表类型
    re.split(pattern, string, maxsplit=0, flags=0)
- re.finditer() 搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象
    re.finditer(pattern, string, flags=0)
- re.sub() 在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串

    - re.sub(pattern, repl, string, count=0, flags=0)
    - flags : 正则表达式使用时的控制标记：
        - re.I --> re.IGNORECASE : 忽略正则表达式的大小写，[A‐Z]能够匹配小写字符
        - re.M --> re.MULTILINE : 正则表达式中的^操作符能够将给定字符串的每行当作匹配开始
        - re.S --> re.DOTALL : 正则表达式中的.操作符能够匹配所有字符，默认匹配除换行外的所有字符
        
## re库的另一种等价用法（编译）
regex = re.compile(pattern, flags=0)：将正则表达式的字符串形式编译成正则表达式对象

## re库的贪婪匹配和最小匹配
- .* Re库默认采用贪婪匹配，即输出匹配最长的子串
- *? 只要长度输出可能不同的，都可以通过在操作符后增加?变成最小匹配
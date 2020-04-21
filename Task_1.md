# 一. HTTP的请求方法
1. GET：向指定的资源发出“显示”请求。GET方法应该只用于读取数据，而不应当被用于“副作用”的操作中（例如在Web Application中）。其中一个原因是GET可能会被网络蜘蛛等随意访问。

2. HEAD：与GET方法一样，都是向服务器发出直顶资源的请求，只不过服务器将不会出传回资源的内容部分。它的好处在于，使用这个方法可以在不必传输内容的情况下，将获取到其中“关于该资源的信息”（元信息或元数据）。

3. POST：向指定资源提交数据，请求服务器进行处理（例如提交表单或者上传文件）。数据被包含在请求文本中。这个请求可能会创建新的资源或修改现有资源，或二者皆有。

4. PUT：向指定资源位置上传输最新内容。

5. DELETE：请求服务器删除Request-URL所标识的资源，或二者皆有。

6. TRACE：回显服务器收到的请求，主要用于测试或诊断。

7. OPTIONS：这个方法可使服务器传回该资源所支持的所有HTTP请求方法。用“*”来代表资源名称向Web服务器发送OPTIONS请求，可以测试服务器共能是否正常。

8. CONNECT：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。通常用于SSL加密服务器的连接（经由非加密的HTTP代理服务器）。方法名称是区分大小写的。当某个请求所针对的资源不支持对应的请求方法的时候，服务器应当返回状态码405（Method Not Allowed），当服务器不认识或者不支持对应的请求方法的时候，应当返回状态码501（Not Implemented）

# 二. Chorme开发者模式提供的工具
1. Elements：允许用户从浏览器的角度来观察网页，用户可以借此看到Chrome渲染页面所需要的HTML、CSS和DOM（Document Object Model）对象。

2. Network：可以看到网页向服务气请求了哪些资源、资源的大小以及加载资源的相关信息。此外，还可以查看HTTP的请求头、返回内容等。

3. Source：即源代码面板，主要用来调试JavaScript。

4. Console：即控制台面板，可以显示各种警告与错误信息。在开发期间，可以使用控制台面板记录诊断信息，或者使用它作为shell在页面上与JavaScript交互。

5. Performance：使用这个模块可以记录和查看网站生命周期内发生的各种事情来提高页面运行时的性能。

6. Memory：这个面板可以提供比Performance更多的信息，比如跟踪内存泄漏。

7. Application：检查加载的所有资源。

8. Security：即安全面板，可以用来处理证书问题等。

- 通过切换设备模式可以观察网页在不同设备上的显示效果，快捷键为：Ctrl + Shift + M（或者在 Mac上使用 Cmd + Shift + M）
- Ctrl+Shift+C可以快速的查看网页位置在源码中位置

# 三. 实战（利用金山词霸翻译爬取的Python之禅）

```python
import requests
def translate(word):
    url="http://fy.iciba.com/ajax.php?a=fy"

    data={
        'f': 'auto',
        't': 'auto',
        'w': word,
    }
    
    headers={
        'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36',
    }#User-Agent会告诉网站服务器，访问者是通过什么工具来请求的，如果是爬虫请求，一般会拒绝，如果是用户浏览器，就会应答。
    response = requests.post(url,data=data,headers=headers)     #发起请求
    json_data=response.json()   #获取json数据
    #print(json_data)
    return json_data
    
def run(word):    
    result = translate(word)['content']['out']   
    print(result)
    return result

def main():
    with open('zon_of_python.txt') as f:
        zh = [run(word) for word in f]

    with open('zon_of_python_zh-CN.txt', 'w') as g:
        for i in zh:
            g.write(i + '\n')
            
if __name__ == '__main__':
    main() 
```
# 四. 实战（爬取豆瓣电影）

```python
import requests
import os

if not os.path.exists('image'):
     os.mkdir('image')

def parse_html(url):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36"}
    res = requests.get(url, headers=headers)
    text = res.text
    item = []
    for i in range(25):
        text = text[text.find('alt')+3:]
        item.append(extract(text))
    return item
       
def extract(text):
    text = text.split('"')
    name = text[1]
    image = text[3]
    return name, image

def write_movies_file(item, stars):
    print(item)
    with open('douban_film.txt','a',encoding='utf-8') as f:
        f.write('排名：%d\t电影名：%s\n' % (stars, item[0]))
    r = requests.get(item[1])
    with open('image/' + str(item[0]) + '.jpg', 'wb') as f:
        f.write(r.content)
        
def main():
    stars = 1
    for offset in range(0, 250, 25):
        url = 'https://movie.douban.com/top250?start=' + str(offset) +'&filter='
        for item in parse_html(url):
            write_movies_file(item, stars)
            stars += 1

if __name__ == '__main__':
    main()
```


<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">知识共享署名-相同方式共享 4.0 国际许可协议</a>进行许可。
import re
import urllib.request
from io import BytesIO
import gzip
import time


def GetNovel():
    time.sleep(1)
    html = urllib.request.urlopen("http://www.xbiquge.la/10/10489/").read()
    #解压字符
    buff = BytesIO(html)
    f = gzip.GzipFile(fileobj=buff)
    html = f.read().decode('utf-8')
    '''
    html = html.decode("utf-8")
    UnicodeDecodeError: 'utf-8' codec can't decode byte 0x8b in position 1: invalid start byte
    需要再次解压才是正常的utf-8
    '''
    #print(html)
    #<dd><a href='/10/10489/9690361.html' >第六章 麻烦大了</a></dd>
    #IndentationError: unindent does not match any outer indentation level  代码没有对齐
    reg = r"<dd><a href='(.*?)' >(.*?)</a></dd>"
    reg = re.compile(reg)     #可添加可不添加，增加效率
    urls = re.findall(reg,html)
    #print(urls[0])
    for url in urls:
        #print(i)
        #http://www.xbiquge.la/10/10489/9690361.html
        ##在实际运行中，发现了新问题
        ##有的章节需要再次解压，有的不需要，我这里使用异常处理来解决
        try:
            time.sleep(1)
            chapterUrl = "http://www.xbiquge.la" + url[0] #因为这个网站解码出来的网址没有前缀，所以需要修改，根据实际情况来
            chapterTitle = url[1]
            print(chapterTitle)
            chapterHtml = urllib.request.urlopen(chapterUrl).read()
            chapterBuff = BytesIO(chapterHtml)
            CF = gzip.GzipFile(fileobj=chapterBuff)
            chapterHtml = CF.read().decode("utf-8")
        except EnvironmentError:
            time.sleep(1)
            chapterUrl = "http://www.xbiquge.la" + url[0]
            chapterTitle = url[1]
            print(chapterTitle)
            chapterHtml = urllib.request.urlopen(chapterUrl).read()
            chapterHtml = chapterHtml.decode("utf-8-sig")
        #print(chapterHtml)
        #匹配内容 开始标签<div id="content"> 结束标签<p>
        Creg = r'<div id="content">(.*?)<p>'
        Creg = re.compile(Creg,re.S)
        chapterContent = re.findall(Creg,chapterHtml)
        for content in chapterContent:               
            content = content.replace("&nbsp;&nbsp;&nbsp;&nbsp;","")  #把"&nbsp;"字符全都替换为""
            content = content.replace("<br />","")      #把"<br/>"字符全部替换为""
            #print(content)
            txt = open('test.txt','a')
            txt = txt.write(chapterTitle + "\n" + content + "\n")

GetNovel() 

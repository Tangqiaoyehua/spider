#coding=utf-8


  
import urllib2
import re


#第一个面向对象的模式设计，糗事百科的段子爬虫
class QSBK:
    
    #初始化
    def __init__(self):
        self.pageIndex=1
        self.user_agent='Mozilla/5.0 (Windows NT 6.1; WOW64; rv:49.0) Gecko/20100101 Firefox/49.0'
        self.headers={'User-Agent':self.user_agent}
        #html内容存储
        self.stories=[]
        self.enable=False

    #从服务器获得html内容
    def getPage(self,pageIndex):
        try:
            url="http://www.qiushibaike.com/8hr/page/"+str(pageIndex)
            request=urllib2.Request(url,headers=self.headers)
            response=urllib2.urlopen(request)
            pagecode=response.read().decode('utf-8')
            return pagecode

        except urllib2.URLErro,e:
            if hasattr(e,"reason"):
                print "连接失败",e.reason
                return None
    #正则表达式匹配html内容
    def getPageItems(self,pageIndex):
        pagecode=self.getPage(pageIndex)
        if not pagecode:
            print"页面加载失败"
            return None

        pattern=re.compile('<h2>(.*?)</h2>.*?<div.*?class="content">.*?<span>(.*?)</span>.*?<i class="number">(.*?)</i>',re.S)
        items=re.findall(pattern,pagecode)
        #存储匹配内容
        pagestories=[]
        for item in items:
            pagestories.append([item[0].strip(),item[1].strip(),item[2].strip()])
        return pagestories
    #加载页数
    def LoadPage(self):
        if self.enable==True:
            #加载下一页
            if len(self.stories)<2:
                pagestories=self.getPageItems(self.pageIndex)
                if pagestories:
                    self.stories.append(pagestories)
                    self.pageIndex+=1
    #把一页的段子分成一个一个，回车表示下一个
    def getOneStory(self,pagestories,page):
        for story in pagestories:
            input=raw_input()
            self.LoadPage()
            if input=='Q':
                self.enable=False
                return
            print u"第%d页\n发言人：%s\n  %s\n---%s人赞"%(page,story[0],story[1],story[2])

    def start(self):
        print u"you are reading a story,Enter for next one,'Q'for quit:"
        
        self.enable=True
        self.LoadPage()
        nowPage=0
        while self.enable:
            if len(self.stories)>0:
                pageStories = self.stories[0]
                #当前读到的页数加一
                nowPage += 1
                #将全局list中第一个元素删除，因为已经取出
                del self.stories[0]
                #输出该页的段子
                self.getOneStory(pageStories,nowPage)
 
spider = QSBK()
spider.start()


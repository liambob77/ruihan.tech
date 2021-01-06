# Robots.txt  

**Robots.txt**是个TXT文件用来告诉搜索引擎哪些页面可以抓取，哪些页面不能抓取。
1994年，REP协议（Robots Exclusion Standard Protocol）诞生，REP协议规定了网络搜索引擎爬虫行为首先访问网站根目录下robots.txt文件。

在网站根目录下放一个robots.txt文本文件（如 <https://ruihan.tech/robots.txt> ），里面可以指定不同的网络爬虫能访问的页面和禁止访问的页面，指定的页面由正则表达式表示。网络爬虫在采集这个网站之前，首先获取到这个robots.txt文本文件，然后解析到其中的规则，然后根据规则来采集网站的数据。

## Robots协议规则

user-agent: denotes the name of the crawler (the names can be found in the Robots Database)
disallow: prevents crawling of certain files, directories or web pages
allow: overwrites disallow and allows crawling of files, web pages, and directories
sitemap (optional): shows the location of the sitemap
*: stands for any number of character
$: stands for the end of the line

## Robots example

```bash
User-agent: *
Disallow: /login/ # 禁止访问特定目录
Disallow: /card/
Disallow: /fotos/
Disallow: /temp/
Disallow: /search/
Disallow: /*.pdf$
Disallow: / # 禁止访问

Sitemap: https://www.example.com/sitemap.xml
```

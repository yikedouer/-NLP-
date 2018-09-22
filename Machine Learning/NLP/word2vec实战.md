## 搜狗语料
转码 用到iconv
- 下载iconv.exe
- 对分散的语料，也就是n个文件，进入语料文件夹，对所有语料在cmd下执行
```
for /r  sougou  %i in (*.txt) do iconv.exe -f GBK -t UTF-8 -c %i > %~ni_corpus.txt
```
sougou是语料存放文件夹，-f是源(from)，-t是目标(to)，-c是忽略未知字符高
- 对于一个.dat文件，执行

```
iconv -f gbk -t utf-8 -c  news_sohusite_xml.dat |find "<content>" > souhu_corpus.txt
```


使用CSS選擇器用法
response.css("title") *輸入title代表css選擇器會開始搜索名為title的元素*
[<Selector query='descendant-or-self::title' data='<title>Quotes to Scrape</title>'>] *這段包含兩個部分可分為前段的Selector query='descendant-or-self::title'，'descendant-or-self::title'表示查詢所有包含"title"的元素;後段的data='<title>Quotes to Scrape</title>'代表"title"元素的標籤和文本內容。*
response.css("title::text").getall() *使用 title::text 表示只獲取"title"內的文本內容，getall代表獲得全部元素*
['Quotes to Scrape']
response.css("title").getall() *這種形式代表可以獲得html形式的標籤*
['<title>Quotes to Scrape</title>']
response.css("title::text").get() *跟getall的差別在於get獲取的是單個元素，而getall獲得的是多個元素，所以會是一個列表[]*
'Quotes to Scrape'
response.css("title::text")[0].get() *表示獲取列表中的第一個元素*
'Quotes to Scrape'
response.css("noelement")[0].get()
Traceback (most recent call last):
...
IndexError: list index out of range *當CSS搜索器沒有搜尋到相應的元素時，就會顯示"list out of range"*
response.css("title::text").re(r"Quotes.*") #此表示使用正規表達式，re()為參數，r"Quotes"的用意為匹配開頭為"Quotes"的文本，而".*"表示後面可以跟隨任意數量的字符
['Quotes to Scrape']
response.css("title::text").re(r"Q\w+") *此用法表示匹配Q開頭的單字，且"\W+"表示匹配任意數量的字母、數字、或符號*
['Quotes']
response.css("title::text").re(r"(\w+) to (\w+)") *"(r"(\w+) to (\w+)")" 這是一個分組用法，中間的to代表連接，所以是個word to word 的用法，以這個爬蟲來說就抓到了"Quotes"和"Scrape"*
['Quotes', 'Scrape']

使用XPATH路徑用法
response.xpath("//title") *使用"//title"可以得到所有帶有"title"的元素*
[<Selector query='//title' data='<title>Quotes to Scrape</title>'>] *此輸出可以分為兩個部分，前半部分Selector query='//title'表示查詢所有包含"title"的元素，而後半部分data='<title>Quotes to Scrape</title>'則是Selecter相對應的html，而<title>裡的是標籤及文本內容。*
response.xpath("//title/text()").get() *加上"text"可以只獲取文本內容*
'Quotes to Scrape'

透過找尋<div>來進行搜索
response.css("div.quote") *透過找尋<div>來獲得標籤，且這些div裡需要有"class=quote"*
[<Selector query="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' quote ')]" data='<div class="quote" itemscope itemtype...'>,
<Selector query="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' quote ')]" data='<div class="quote" itemscope itemtype...'>,...]

quote = response.css("div.quote")[0]
text = quote.css("span.text::text").get() *使用css選擇器找尋所有包含"quote"的元素，並且要符合"span.text::text"的條件，"span.text::text"表示查詢所有class帶有text且在<span>裡的元素*
author = quote.css("small.author::text").get()

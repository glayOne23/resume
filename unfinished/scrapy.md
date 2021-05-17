# 1 and 2 Introduction
- The name of program that used to scrap data -> spider
- Every web page is made up of 2 main languages : html and css
- How to  extract data :
    1. right click the word that u want to scrap -> inspect element
    2. u must find a unique property related to the element that u want to scrub
- Example :
    1. Web page
        ![](img_scrapy/scrap1.png?raw=true)
    2. Code :
        ```python
        import scrapy

        class CoronaIndoSpider(scrapy.Spider):
            name = 'coronaindospider'
            start_urls = ['https://corona.jatengprov.go.id/']

            def parse(self, response):
                for content in response.css('div.card'):
                    yield {
                        'header': content.css('div.card-header ::text').get(),
                        'jumlah': content.css('div.card-body > p.card-text ::text').get(),
                        # 'keterangan' : content.css('div.card-body').get()
                    }
        ```
    3. Run : `scrapy runspider [file_name.py]`
    4. Result :
        ```jsp
        2020-03-22 15:05:42 [scrapy.core.scraper] DEBUG: Scraped from <200 https://corona.jatengprov.go.id/>
        {'header': 'Orang Dalam Pemantauan (ODP)', 'jumlah': '2.416'}
        2020-03-22 15:05:42 [scrapy.core.scraper] DEBUG: Scraped from <200 https://corona.jatengprov.go.id/>
        {'header': 'Pasien Dalam Pengawasan (PDP)', 'jumlah': '165'}
        2020-03-22 15:05:42 [scrapy.core.scraper] DEBUG: Scraped from <200 https://corona.jatengprov.go.id/>
        {'header': 'Kasus ', 'jumlah': '14'}
        ```

# 3 Robot.txt
- U can check what site you is allowed to scrap the website by visit robot.txt : `<site-url>/text`
- If u want obey robot.txt, in setting.py : change from false to True :  `ROBOTSTXT_OBEY = False`

# 4 and 5 Make Scrap Project
u can make a project of spider (with all setting not only file.py) by using this step :
1. `python3 -m venv env`
2. `source env/bin/activate`
3. `scrapy startproject [folder_name]`

# 7 and 8 Make First Scrap
- Steps :
    - make new project
    - make new .py file inside spiders dir, name it what u want in this case quote_spider.py
    - Code
        ```python
        import scrapy

        class QuoteSipder(scrapy.Spider):
            # name of our spider
            name = 'quotes'
            # list of url u want to scrap
            start_urls = [
                'http://quotes.toscrape.com/'
            ]

            # response contains all source code of the website from start_urls
            def parse(self, response):
                title_tab = response.css('title ::text').extract()
                yield {
                    'title_tab' : title_tab
                }
        ```
    - Run : `scrapy crawl quotes` . quotes is name of your spider that u named it in code
    - Result :
        ```jsp
        {'title_tab': ['Quotes to Scrape']}
        ```
    - If we use get() instead of extract the value will not in dictionary
        ```jsp
        {'title_tab': 'Quotes to Scrape'}
        ```
- yield in python function is like return, but the different is the function in yield will return a generator : https://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do
    - Keyword for yield : iteration, generator, not in memory

# 9 CSS selector in Shell (breakdown the code
- Extensions to CSS Selectors :
    - to select text nodes, use ::text
    - to select attribute values, use ::attr(name) where name is the name of the attribute that you want the value of
    - **Warning** : These pseudo-elements are Scrapy-/Parsel-specific. They will most probably not work with other libraries like lxml or PyQuery.
```shell
$ scrapy shell "http://quotes.toscrape.com/"
>>> response.css('title')
[<Selector xpath='descendant-or-self::title' data='<title>Quotes to Scrape</title>'>]
>>> response.css('title').extract()
['<title>Quotes to Scrape</title>']
>>> response.css('title::text').extract()
['Quotes to Scrape']
>>> response.css('title::text')[0].extract()
'Quotes to Scrape'
>>> response.css('title::text').extract_first()
'Quotes to Scrape'
>>> response.css('span.text::text').extract()
['“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”', '“It is our choices, Harry, that show what we truly are, far more than our abilities.”', '“There are only two ways to live your life. One is as though nothing is a miracle. The other is as though everything is a miracle.”', '“The person, be it gentleman or lady, who has not pleasure in a good novel, must be intolerably stupid.”', "“Imperfection is beauty, madness is genius and it's better to be absolutely ridiculous than absolutely boring.”", '“Try not to become a man of success. Rather become a man of value.”', '“It is better to be hated for what you are than to be loved for what you are not.”', "“I have not failed. I've just found 10,000 ways that won't work.”", "“A woman is like a tea bag; you never know how strong it is until it's in hot water.”", '“A day without sunshine is like, you know, night.”']
>>> response.css('span.text::text')[1].extract()
'“It is our choices, Harry, that show what we truly are, far more than our abilities.”'
>>> response.css('.author::text').extract()
['Albert Einstein', 'J.K. Rowling', 'Albert Einstein', 'Jane Austen', 'Marilyn Monroe', 'Albert Einstein', 'André Gide', 'Thomas A. Edison', 'Eleanor Roosevelt', 'Steve Martin']
>>> response.css('.author::text').get()
'Albert Einstein'
```
- Chrome extension to easily get css selector from web page : SelectorGadget

# 10 Xpath Selector
- Scrapy comes with its own mechanism for extracting data. They’re called selectors because they “select” certain parts of the HTML document specified either by XPath or CSS expressions.
- XPath is a language for selecting nodes in XML documents, which can also be used with HTML. https://www.w3schools.com/xml/xpath_syntax.asp
- CSS is a language for applying styles to HTML documents. It defines selectors to associate those styles with specific HTML elements.
- U can use css selector or xpath selector only or combine them
```python
# First Comparasion
>>> response.css("title ::text").extract()
['Quotes to Scrape']
>>> response.xpath("//title /text()").extract()
['Quotes to Scrape']
# Second Comparasion
>>> response.css("span.text ::text").extract()
['“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”', '“It is our choices, Harry, that show what we truly are, far more than our abilities.”', '“There are only two ways to live your life. One is as though nothing is a miracle. The other is as though everything is a miracle.”', '“The person, be it gentleman or lady, who has not pleasure in a good novel, must be intolerably stupid.”', "“Imperfection is beauty, madness is genius and it's better to be absolutely ridiculous than absolutely boring.”", '“Try not to become a man of success. Rather become a man of value.”', '“It is better to be hated for what you are than to be loved for what you are not.”', "“I have not failed. I've just found 10,000 ways that won't work.”", "“A woman is like a tea bag; you never know how strong it is until it's in hot water.”", '“A day without sunshine is like, you know, night.”']
>>> response.xpath("//span[@class='text'] /text()").extract()
['“The world as we have created it is a process of our thinking. It cannot be changed without changing our thinking.”', '“It is our choices, Harry, that show what we truly are, far more than our abilities.”', '“There are only two ways to live your life. One is as though nothing is a miracle. The other is as though everything is a miracle.”', '“The person, be it gentleman or lady, who has not pleasure in a good novel, must be intolerably stupid.”', "“Imperfection is beauty, madness is genius and it's better to be absolutely ridiculous than absolutely boring.”", '“Try not to become a man of success. Rather become a man of value.”', '“It is better to be hated for what you are than to be loved for what you are not.”', "“I have not failed. I've just found 10,000 ways that won't work.”", "“A woman is like a tea bag; you never know how strong it is until it's in hot water.”", '“A day without sunshine is like, you know, night.”']
# Combination
>>> response.css("li.next a").xpath("@href").extract()
['/page/2/']
```

# 11
```python
import scrapy

class QuoteSipder(scrapy.Spider):
    name = 'quotes'
    start_urls = [
        'http://quotes.toscrape.com/'
    ]

    def parse(self, response):
        all_div_quotes = response.css('div.quote')

        for content in all_div_quotes :
            # get() to get only one data
            title = content.css('span.text ::text').get()
            author = content.css('.author ::text').get()
            # extract() to get all data in list
            tags = content.css('.tag ::text').extract()

            yield {
                'title' : title,
                'author' : author,
                'tags' : tags
            }
```
- Result :
```
{'title': '“The person, be it gentleman or lady, who has not pleasure in a good novel, must be intolerably stupid.”', 'author': 'Jane Austen', 'tags': ['aliteracy', 'books', 'classic', 'humor']}
2020-03-25 05:44:13 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'title': "“Imperfection is beauty, madness is genius and it's better to be absolutely ridiculous than absolutely boring.”", 'author': 'Marilyn Monroe', 'tags': ['be-yourself', 'inspirational']}
2020-03-25 05:44:13 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'title': '“Try not to become a man of success. Rather become a man of value.”', 'author': 'Albert Einstein', 'tags': ['adulthood', 'success', 'value']}
2020-03-25 05:44:13 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'title': '“It is better to be hated for what you are than to be loved for what you are not.”', 'author': 'André Gide', 'tags': ['life', 'love']}
2020-03-25 05:44:13 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'title': "“I have not failed. I've just found 10,000 ways that won't work.”", 'author': 'Thomas A. Edison', 'tags': ['edison', 'failure', 'inspirational', 'paraphrased']}
2020-03-25 05:44:13 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'title': "“A woman is like a tea bag; you never know how strong it is until it's in hot water.”", 'author': 'Eleanor Roosevelt', 'tags': ['misattributed-eleanor-roosevelt']}
2020-03-25 05:44:13 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
{'title': '“A day without sunshine is like, you know, night.”', 'author': 'Steve Martin', 'tags': ['humor', 'obvious', 'simile']}
```

# 12 Items
- Item is container
- Why exactly do we need to put extracted data in containers ? because they have already extracted the data can't we just put them in some kind of a database directly ? the answer is yes obviously you can, but there might be a few problems when you are storing the data directly inside the database when you are working on big project due to Python dicts lack structure: it is easy to make a typo in a field name or return inconsistent data,
- Plot : **Extracted data -> Temporary containers (items) -> Storing in JSON/XML/CSV**
- Steps :
1. in items.py :
    ``` python
    import scrapy

    class LearnScrapItem(scrapy.Item):
        # define the fields for your item here like:
        title = scrapy.Field()
        author = scrapy.Field()
        tags = scrapy.Field()
    ```
2. in spiders/quotes_spider.py :
    ```python
    import scrapy
    # import container
    from ..items import LearnScrapItem


    class QuoteSipder(scrapy.Spider):
        name = 'quotes'
        start_urls = [
            'http://quotes.toscrape.com/'
        ]

        def parse(self, response):
            # make instance of container        
            items = LearnScrapItem()

            all_div_quotes = response.css('div.quote')

            for content in all_div_quotes :
                title = content.css('span.text ::text').get()
                author = content.css('.author ::text').get()
                tags = content.css('.tag ::text').extract()

                # put extracted data to container
                items['title'] = title
                items['author'] = author
                items['tags'] = tags

                # yield items
                yield items
    ```
- Result :
    ```
    {'author': 'Albert Einstein',
    'tags': ['change', 'deep-thoughts', 'thinking', 'world'],
    'title': '“The world as we have created it is a process of our thinking. It '
            'cannot be changed without changing our thinking.”'}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'J.K. Rowling',
    'tags': ['abilities', 'choices'],
    'title': '“It is our choices, Harry, that show what we truly are, far more '
            'than our abilities.”'}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'Albert Einstein',
    'tags': ['inspirational', 'life', 'live', 'miracle', 'miracles'],
    'title': '“There are only two ways to live your life. One is as though '
            'nothing is a miracle. The other is as though everything is a '
            'miracle.”'}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'Jane Austen',
    'tags': ['aliteracy', 'books', 'classic', 'humor'],
    'title': '“The person, be it gentleman or lady, who has not pleasure in a '
            'good novel, must be intolerably stupid.”'}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'Marilyn Monroe',
    'tags': ['be-yourself', 'inspirational'],
    'title': "“Imperfection is beauty, madness is genius and it's better to be "
            'absolutely ridiculous than absolutely boring.”'}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'Albert Einstein',
    'tags': ['adulthood', 'success', 'value'],
    'title': '“Try not to become a man of success. Rather become a man of value.”'}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'André Gide',
    'tags': ['life', 'love'],
    'title': '“It is better to be hated for what you are than to be loved for '
            'what you are not.”'}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'Thomas A. Edison',
    'tags': ['edison', 'failure', 'inspirational', 'paraphrased'],
    'title': "“I have not failed. I've just found 10,000 ways that won't work.”"}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'Eleanor Roosevelt',
    'tags': ['misattributed-eleanor-roosevelt'],
    'title': '“A woman is like a tea bag; you never know how strong it is until '
            "it's in hot water.”"}
    2020-03-25 06:51:12 [scrapy.core.scraper] DEBUG: Scraped from <200 http://quotes.toscrape.com/>
    {'author': 'Steve Martin',
    'tags': ['humor', 'obvious', 'simile'],
    'title': '“A day without sunshine is like, you know, night.”'}
    ```

# 13 Storing data in JSON, XML, and CSV
- `scrapy crawl quotes -o quotes.json`
- `scrapy crawl quotes -o quotes.xml`
- `scrapy crawl quotes -o quotes.csv`

# 14 Pipelines
- Each item pipeline component (sometimes referred as just “Item Pipeline”) is a Python class that implements a simple method. They receive an item and perform an action over it, also deciding if the item should continue through the pipeline or be dropped and no longer processed.
- Typical uses of item pipelines are:
    - cleansing HTML data
    - validating scraped data (checking that the items contain certain fields)
    - checking for duplicates (and dropping them)
    - storing the scraped item in a database
- Usually is used to storing data in db : **Extracted data -> Temporary containers (items) -> Storing in database**
- To use pipelines enable it in setting.py, and pipelines.py **automatically will be called when yield is called**
    ```python
    ITEM_PIPELINES = {
    'learn_scrap.pipelines.LearnScrapPipeline': 300,
    }
    ```
    - 300 is value to determine order of pipeline if u have more than one pipeline: items go through from lower valued to higher valued classes. It’s customary to define these numbers in the 0-1000 range.
# 15 Sqlite3 Basic
- Steps :
    - make `database.py` for trying :
        ```python
        import sqlite3

        # connect or create sqlite db
        conn = sqlite3.connect('myquote.db')
        curr = conn.cursor()

        # make new table
        curr.execute("""
            create table quotes(
            title text, 
            author text, 
            tags text
            )"""
        )

        conn.commit()
        conn.close()
        ```
    - `python3 database.py` will create new sqlite db with the name myquote.db and has one table with the name qoutes
# 16 Storing Data in Sqlite3
- in pipelines.py :
    ```python
    import sqlite3

    class LearnScrapPipeline(object):

        # constructor
        def __init__(self):
            self.create_connection()
            self.create_table()

        def create_connection(self):
            self.conn = sqlite3.connect('myquotes.db')
            self.curr = self.conn.cursor()

        def create_table(self):
            self.curr.execute("""drop table if exists quotes""")
            self.curr.execute("""create table quotes(
                                title text, 
                                author text, 
                                tags text
                                )"""
                            )

        # process_item() is automatically called when using pipelines.py without using __init__()
        def process_item(self, item, spider):
            self.store_db(item)
            # return is used for printing result in command line
            return item

        def store_db(self, item):
            self.curr.execute("""insert into quotes values (?,?,?)""", (
                                    item['title'],
                                    item['author'],
                                    item['tags'][0]
                                )
                            )
            self.conn.commit()
    ```
# 17 Storing data in MySQL DB
- Steps : 
1. `pip install mysql-connector-python`
2. in pipelines.py
    ```python
    import mysql.connector
    # for tags
    import json

    class LearnScrapPipeline(object):

        # constructor
        def __init__(self):
            self.create_connection()
            self.create_table()

        def create_connection(self):
            self.conn = mysql.connector.connect(
                host = 'localhost',
                user = 'root',
                password = '',
                database = 'test_scrap'
            )
            self.curr = self.conn.cursor()

        def create_table(self):
            self.curr.execute("""drop table if exists quotes""")
            self.curr.execute("""create table quotes(
                                title text, 
                                author text, 
                                tags text
                                )"""
                            )

        # process_item() is automatically called when using pipelines.py without using __init__()
        def process_item(self, item, spider):
            self.store_db(item)
            # return is used for printing result in command line
            return item

        def store_db(self, item):
            self.curr.execute("""insert into quotes values (%s,%s,%s)""", (
                                    item['title'],
                                    item['author'],
                                    # change mutiple value into json format
                                    json.dumps(item['tags'])
                                )
                            )
            self.conn.commit()
    ```





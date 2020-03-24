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

# 9 CSS selector in Shell (breakdown the code)
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


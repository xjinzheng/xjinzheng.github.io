---
title: "Stock News Mining"
collection: project
permalink: /project/2018-stocknews
excerpt: "Course Project, [IM2011] Programming for Business Computing, NTU, Spring 2018"
---

[IM2011] Programming for Business Computing, NTU, Spring 2018

In out final project, we made a UI programe that allows the users to type in the company's keywords and the data range of the stock news they're interested in.
The programe will generate the stock price info 

Keywords: Stock News, Sentiment, Text Analysis



```python

def get_GoogleSearch_link():
    company = 'amazon'
    start_date = '04/01/2018'
    end_date = '04/06/2018'
    word = 'site://www.bloomberg.com/news/articles' + '+' + company
    search_result_link = "https://www.google.com/search?q=%s&newwindow=1&tbs=cdr:1,cd_min:%s,cd_max:%s,sbd:1&tbm=nws&source=lnt&sa=X&ved=0ahUKEwjc0pn2r93bAhVHvrwKHcqfCIsQpwUIHQ&biw=1440&bih=803&dpr=2" \
                    % (word, start_date, end_date)
    return(search_result_link)


browser = webdriver.Chrome(executable_path='/usr/local/bin/chromedriver')
all_links = []


def get_all_pages_links(page_link):

    browser.get(page_link)
    source = browser.page_source
    soup = BeautifulSoup(source, "html.parser")
    items = soup.findAll("h3")


    for item in items:
        href = item.find("a").get("href")
        all_links.append(href)
        print(href)

    next_page = soup.find('a', class_='pn', id='pnnext')

    if next_page:
        next_page_url = next_page.get('href')
        next_page_link = 'http://www.google.com' + next_page_url
        get_all_pages_links(next_page_link)
    else:
        browser.close()

    return all_links


def parse_article(url):

    browser.get(url)
    source = browser.page_source
    soup = BeautifulSoup(source, "html.parser")
    browser.close()

    divs1 = soup.find_all('div', class_="body-copy-v2 fence-body")
    divs2 = soup.find_all('div', class_="body-copy fence-body")
    divs = divs1 + divs2

    link_date = str(url)[40:50]

    content = []
    content.append(link_date)
    article = ''
    for div in divs:
        for p in div.select('p')[:-2]:
            passage = p.text.strip()
            article += passage
        content.append(article)
    print(content)


url = 'https://www.bloomberg.com/news/articles/2018-04-05/urgent-minnesota-s-pawlenty-seeks-return-to-governor-s-mansion'
parse_article(url)

```

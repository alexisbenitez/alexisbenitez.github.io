---
layout: post
title: "Data scraping"
subtitle: "A scraper to get all laptops on flipkart.com"
date: 2020-06-20 23:45:13 -1300
background: '/img/posts/web-scraping/python-scraping.png'
---

# Data Scraping

In short, data scraping is a technique where a computer program extracts data from human-readable output coming from another program.

<br>
<hr>
<br>

***Getting laptops list from flipkart.com***<br><br>

The goal of this post is to show how I create a Data Frame with all the laptops posted on flipkart.com online store. For this I used Pandas, Beatiful Soup and Regex modules;
I have also created a function *getPageData()* to get all the data from the links and looped through them, appending the new data to the product object to end up converting it into the final data frame.


***Importing libraries:***

```python
import re
import requests
import pandas as pd
from requests import get
from bs4 import BeautifulSoup
```

***Getting the num of pages the search returns:***<br>
Here I get a site response via *requests.get()* and store it within the object *page*.<br>
Then I parse it on *soup* to use findAll and get the element where the num of pages is.

```python
link = 'https://www.flipkart.com/search?q=laptop&otracker=search&otracker1=search&marketplace=FLIPKART&as-show=on&as=off'
page = requests.get(link)
soup = BeautifulSoup(page.text)
element = soup.findAll('div', attrs = {'class': '_2MImiq'})[0]
numOfPages = re.findall('(\d+)(?!.*\d+)', element.find('span').text)
numOfPages = int(numOfPages[0])
```

***Generate list of links to scrap:***<br>
I create an array to store all the links to go through. First element is different so it's added before the loop.

```python
pageLinks = []
pageLinks.append(link)
for pageNum in range(2, numOfPages + 1):
    pageLinks.append(link + '&page=' + str(pageNum))
```

***Initialize cleared products' list:***

```python
productsList = []
productsList.clear()
```


***Function to get data from one page:***<br>
This function takes the obj *soupPage* which is going to be each parsed page containing the laptops.

```python
def getPageData(soupPage):
    for element in soupPage.findAll('div', attrs = {'class': '_3pLy-c row'}):
        name = element.find('div', attrs = {'class': '_4rR01T'})
        price = element.find('div', attrs = {'class': '_30jeq3 _1_WHN1'})
        product = {'Product': name.text,
           'Price': price.text[1:].replace(',', '') if price is not None else 'N/A'}
        productsList.append(product)
```

***Extract data from pages:***<br>
For loop that calls each page and parses it before passing it to *getPageData* function, described above.

```python
for link in pageLinks:
    print(link)
    page = requests.get(link)
    soupPage = BeautifulSoup(page.text, 'html.parser')  
    getPageData(soupPage)
print("Done!")
```

***Create the Data Frame:***

```python
df = pd.DataFrame(productsList)
```


***Print the Data Set:***

```python
pd.set_option("max_rows", None)
df.style
```

***The final result:***

<img src="/img/posts/web-scraping/flipkart_laptops_df.png">

<br>
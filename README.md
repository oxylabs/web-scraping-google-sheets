
* [What is IMPORTXML](#WhatisIMPORTXML)
* [How to extract data from a website to Google Sheets](#HowtoextractdatafromawebsitetoGoogleSheets)  
* [Other related functions](#Otherrelatedfunctions)
* [Import table from website to Google Sheets](#ImporttablefromwebsitetoGoogleSheets)
* [Import data from XML Feeds to Google Sheets](#ImportdatafromXMLFeedstoGoogleSheets)
* [Customizing data imported by IMPORTFEED](#CustomizingdataimportedbyIMPORTFEED)
* [Importing Data from CSV to Google Sheets](#ImportingDatafromCSVtoGoogleSheets)
* [Does the data stay fresh?](#Doesthedatastayfresh)
* [Common Errors](#common-errors)
* [Errors related to volatile functions](#Errorsrelatedtovolatilefunctions)


Google Sheets can be a very effective tool for web scraping. While most ways of web scraping require you to write code, web scraping with Google Sheets needs no coding and or add on. All you need to do is use a built-in function of Google Sheets.

This guide will give an overview you how to scrape website data with Google Sheets. 

If you want to learn more, see our blog post.

##  <a name='WhatisIMPORTXML'></a>What is IMPORTXML

IMPORTXML is a function that can import data from various data types. 

If you want to extract the title element from the Quotes to Scrape web page, the formula would be as follows:

```
=IMPORTXML("https://quotes.toscrape.com/","//title")
```

As evident here, the first parameter is the web page URL, and the second parameter is the XPath query.

If you want to extract the first quote from the webpage, the formula will be as follows:

```
=IMPORTXML("https://quotes.toscrape.com/","(//[@class='text']/text())[1]")
```



If this XPath query seems like something you are not comfortable with, we recommend reading the XPath section on [our blog](https://oxylabs.io/blog/xpath-vs-css) to learn more about writing XPath queries. 

Alternatively, you can enter the URL in a cell:

```
=IMPORTXML(A1,A2)
```

##  2. <a name='HowtoextractdatafromawebsitetoGoogleSheets'></a>How to extract data from a website to Google Sheets

###  2.1. <a name='Step1:FindXPathforselectingElements'></a>Step 1: Find XPath for selecting Elements

In this example, we will work with https://books.toscrape.com/, and we want to get all the book titles. These requirements mean that we need to write a custom XPath. This Xpath is as follows:

//h3/a/@title

###  <a name='Step2:CreateanewGoogleSheet'></a>Step 2: Create a new Google Sheet

Navigate to [Google Sheets](https://docs.google.com/spreadsheets/u/0/) and create a new sheet. This step requires you to log in to your Google account if you haven't done so already.

###  <a name='Step3:EntertheURLandXPathintwocells'></a>Step 3: Enter the URL and XPath in two cells

Enter the URL of the webpage and the XPath in two cells.

###  <a name='Step4:ExtractWebsiteDataWithGoogleSheets'></a>Step 4: Extract Website Data With Google Sheets

In a new cell, for example, A2, enter the following formula:

```
=IMPORTXML(B1,B2)
```

This formula effectively calls the following function:

```
=IMPORTXML("ttps://books.toscrape.com/","//h3/a/@title")
```



If you want to extract the book prices, the first step is to create the XPath for prices. This XPath would be as follows:

```
//*[@class="price_color"]/text()
```



Enter this XPath in a Cell, let's say, B3. After that, enter the following formula in the cell B4:

```
=IMPORTXML(B1, B3)
```



##  <a name='Otherrelatedfunctions'></a>Other related functions

Apart from IMPORTXML, a few other functions can be used for web scraping directly from Google Sheets:

- IMPORTHTML
- IMPORTFEED
- IMPORTDATA



### <a name='ImporttablefromwebsitetoGoogleSheets'></a>Import table from website to Google Sheets

This function expects three parameters:

- URL
- Either "table" or "list"
- The index of the table or the list you want to scrape.

For example, see [List of highest-grossing films - Wikipedia](https://en.wikipedia.org/wiki/List_of_highest-grossing_films). This page contains the list in a table.

```
=IMPORTHTML(B1,"table",1)
```

For example, if we wanted only the movie titles, which are in column number 3, our formula would be as follows:

```
=INDEX(IMPORTHTML("https://en.wikipedia.org/wiki/List_of_highest-grossing_films","table",1),,3)
```

##  <a name='ImportdatafromXMLFeedstoGoogleSheets'></a>Import data from XML Feeds to Google Sheets

Let's take the example of the [New York Times Technology feeds](https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml) to see this function in action. 

Create a new sheet and enter the url of the feed in cell B1:

https://rss.nytimes.com/services/xml/rss/nyt/Technology.xml

Now in the cell A2, enter the following formula:

```
=IMPORTFEED(B1)
```



##  <a name='CustomizingdataimportedbyIMPORTFEED'></a>Customizing data imported by IMPORTFEED

The IMPORTFEED function has the following optional parameters:

- Query - You can use this to specify which information you want to import. More on this just in a bit.
- Headers - As you can see from the above image, there are no headers in the imported data. If you want to see column headers, then set this parameter to TRUE.
- num_items - You can also control how many items are fetched. If you want only five items to be imported, set this parameter to 5.

Update the function call to the following:

```
=IMPORTFEED(B1,,TRUE,5)
```

If you want only the information about the feed, enter the following formula:

```
=IMPORTFEED(B1,"feed")
```



If you want to get only the titles, enter the following formula:

```
=IMPORTFEED(B1,"items title")
```



##  <a name='ImportingDatafromCSVtoGoogleSheets'></a>Importing Data from CSV to Google Sheets

If you have a URL that contains a CSV file, you can use the IMPORTDATA function to get the data.

For example, create a new sheet and enter the following URL in the cell B1:

https://www2.census.gov/programs-surveys/decennial/2020/data/apportionment/apportionment.csv

In the cell A2, enter the following formula:

```
=IMPORTDATA(B1)
```



## <a name='Doesthedatastayfresh'></a>Does the data stay fresh?

If you keep your google sheet open, these functions check for updated data every hour.

Data will also be refreshed if you delete and add the same cell.

Note that data will not be refreshed if you refresh your sheet.

Data will also not be refreshed if you copy-paste a cell with these functions.

# Common Errors

The following are some of the common errors you may face while creating your web scraping Google Sheet:

##  <a name='Error:Arrayresultwasnotexpanded'></a>Error: Array result was not expanded

Array result was not expanded because it would overwrite data in A36.

This error means you need to make room by adding more cells for the results.

##  <a name='Error:Resulttoolarge'></a>Error: Result too large

The solution is to update the XPath query so that a smaller amount of data is returned. 

##  <a name='Errorsrelatedtovolatilefunctions'></a>Errors related to volatile functions

If you see the following error:

Error: This function is not allowed to reference a cell with NOW(), RAND(), or RANDBETWEEN()

It means that you are trying to reference one of the volatile functions, such as NOW, RAND, or RANDBETWEEN, in one of the parameters. These references may be indirect or direct.



If you want to learn more, see our blog post.

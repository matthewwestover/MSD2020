## Week 3 - Day 1
### Web Scrapping
Lots of data is available on websites  
Website data comes in (mostly) HTML format  
We can get data by just downloading HTML with an HTTP request then extracting the data we care about. Sort of.  
HTML is formatted for humans to look at, not for automatic data extraction/manipulation  
It is a **PITA** and should be a last resort if you need the data. 

#### Scrapping Issues
Not all data is in static HTML. The page might do dynamic things with Javascript before displaying it  
HTML is a format for displaying information, not processing information. It can require a lot of manual work to find out exactly where to what you need and how to extract it  
Some website operators don't like scraping and/or are concerned about DOS attacks. Many sites employ "rate-limiting" or try to detect and block scraping - A cohort 1 student got temporarily blocked from Delta's website for trying to scrap flight info... oops  
OK, so you've figured out how to extract the data you care about from a web page, but then they push a new version of the site with a brand new look... gotta figure out how to extract your data again. Scraping is very brittle

#### Scrapping in Python
Use the BeautifulSoup library  
It reads HTML and has methods very similar to JS's DOM traversal/ manipulation for filtering/processing HTML documents  
Probably a good idea to download the data in one cell and process it in others so you can tweak your processing code without repeatedly making web requests (which may get you temporarily blocked)

#### APIs
For services which allow you to grab/use data from them, they usually provide an "API"  
Often these are "REST" APIs. Typically, you make an HTTP request to a special URL and get back a JSON string  
JSON is a lot easier to work with than HTML because it's designed to be machine readable. Also, the APIs are usually designed for programmers to work with, as opposed to just looking pretty in a browser  
Some APIs provide a library which hides the HTTP calls/JSON decoding behind a nice interface  
Many APIs require you to register and get an "access token" which you provide with your requests (for authorization and usage tracking, etc). This is provided with all the HTTP requests you make. An API with a library can make this easier to work with


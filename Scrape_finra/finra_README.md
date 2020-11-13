**Scrape_finra motivation and usage**

This code uses Selenium to scrape Morningstar's Finra tool which lists bond offering information.

I wanted to be able to see how a particular company's liquidity and solvency risk has changed over time which is the reason why I created this program. The Coronavirus pandemic had made a mess of the debt markets in March and April and I was interested to see if companies were still having a hard time financing through debt or if they were back to normal levels. By accessing the Finra data I can see how a company's bond yields have changed over time for a certain bond offering. I can look for a short-term and long-term bond offering and get a pretty good idea of what the bond market thinks of the company. 

There are only two things that need to be entered for Scrape_finra so I decided to do away with the Google Sheets. In the program, on line 11, enter a target_date. On line 18, change the URL to a Finra Bond offering like [this one.](http://finra-markets.morningstar.com/BondCenter/BondTradeActivitySearchResult.jsp?ticker=C931349&startdate=11%2F12%2F2019&enddate=11%2F12%2F2020) The program will terminate when the target_date is met. If you enter a date that is not listed on the Finra site (this can happen because some bond offerings don't get traded every day), it will terminate at the next applicable date. Then the program will return the date, bond yield, and trade amount for every transaction from the most recent to the trades made on the target_date. It will enter all this data into an Excel file. Below is what was gathered when I entered '9/9/2020' for the target_date and used [this](http://finra-markets.morningstar.com/BondCenter/BondTradeActivitySearchResult.jsp?ticker=C931349&startdate=11%2F12%2F2019&enddate=11%2F12%2F2020) bond offering as the site to be scraped. 

![fin_image_1](https://github.com/rossleavitt/stock_analysis/blob/main/images/finra_img_1.PNG) 

Then I can use the trade amount and the bond yield data that I get for each day and come up with an average bond yield for each of the dates listed. With that I can create graphs to see how these yields have changed over time.

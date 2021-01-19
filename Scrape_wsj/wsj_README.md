**Scrape_wsj motivation and usage**

This program utilizes Google Sheets API for entering in information and Selenium for scraping wsj.com.

The motivation behind creating 'Scrape_wsj' was so that I could have a way of accessing many different companies' financial statement data in a timely manner. I could then use this data to compare similar companies on their use of debt, capital expenditures, cash flow from operations, etc. Below is the user interface I came up with which allows the user to enter the kind of information they want and it is where the program will return the data when it's done scraping.

![table 1](https://github.com/rossleavitt/stock_analysis/blob/main/images/wsj_img_1.PNG) 

If you are looking to use this code yourself here are the steps you should take to ensure ease of use. *Note: green cells are 'user-input' cells and red cells are 'program-return' cells.*

* First, in cell A1 enter either 'Annually' or 'Quarterly' depending on whether you are looking for annual data or quarter data. 

* Second, enter the ticker symbols of the companies you'd like to scrape for in in column A, starting at row 4.

* Third, in the first three rows of the Google Sheet you should enter the information regarding the type of data you want to collect. This consists of the **Statement**, **Time Period**, and **Account**. 
  
  * Here are valid entries for the 'Statement' field: 'income-statement', 'balance-sheet', 'cash-flows'. 

  * For the Time Period row, things get difficult because there is a difference depending on whether you enter 'Annually' or 'Quarterly' into cell A1. If you're looking for annual data then enter the year you would like to scrape for (e.g. 2019). For quarterly data, different companies are on different schedules with respect to when they report. With that in mind, I made it so that the Time Period that is entered is in relation to the most recent quarter (e.g. Most Recent Quarter + 1). For the full key, click [here](supplementary_files/quarter_time_period.md). 

  * For the account row, make sure that the account you enter is listed on the web page for the given financial statement. Fortunately, for companies operating in the same industry, they will likely all have the same account names. *Note: when you are entering titles in 'Account', you can make sure that they exist on the web page you are scraping by going to that financial statement for that company on wsj.com.*
 
* Finally, enter in what I call 'Unique Labels' in column O of the spreadsheet. Any account that will return as a percentage (like Current Ratio) or which has its own set of units (like Earnings per Share) should be included in Unique Labels. All other accounts (like Cash, Free Cash Flow, or Long-Term Debt) will be multiplied by what the web page says are the units for that page. Usually, the units are in thousands of dollars or millions of dollars. Everything in Unique Labels will not be multiplied by that number.

Now that I have gone over the set up, I will show you one example of how this code could be used for financial analysis. After setting up the Google Sheet like I showed previously, I ran the code and after it was done running the red cells were populated. I took this data and came up with the following table for the second quarter of 2020. 

![wsj image 2](https://github.com/rossleavitt/stock_analysis/blob/main/images/wsj_img_2.PNG)

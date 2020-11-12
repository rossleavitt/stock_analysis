# stock_analysis
                                                                    ##Summary of stock_analysis
This GitHub consists of 4 programs which scrape different websites for various kinds of data pertaining to a stock or a group of stocks. All this data was then used in a factor analysis of the Utility Industry. If you would like more information about the factor analysis that was done, visit here (LINK HERE 1). 

Below are short summaries for each of the programs that I made. After the summaries, I will go into more depth about each program, why I made it, and how I used it. 

Scrape_wsj: scrapes financial statement information (either from the balance sheet, income statement, or statement of cash flows) from wsj.com and enters that data into Google Sheets. Can specify between Quarterly and Yearly financial data. 

Scrape_yahoo: scrapes a company's Yahoo Financial Statistics page which offers information regarding that company's valuation ratios, profitability metrics, and other data for that company. 

Scrape_finra: scrapes Morningstar's Finra site so that I can have access to the bond yields of a particular bond offering from a company of interest. It will keep scraping until it reaches a certain predetermined date, giving the user time-series data of bond yields. 

Price_change: uses an API (documentation here (LINK HERE 2)) which returns time-series data of a stock's market price at close.


Scrape_wsj motivation and usage:

The motivation behind creating 'Scrape_wsj' was so that I could have a way of accessing many different companies' financial statement data in a timely manner. I could then use this data to compare similar companies on their use of debt, capital expenditures, cash flow from operations, etc. [Here](images/wsj_img.PNG) is what I came up with as far as telling the program what kind of information I want and where to put that information when it's done scraping.

If you are looking to use this code yourself here are the steps you should take to ensure ease of use.

First, in cell A1 enter either 'Annually' or 'Quarterly' depending on whether you are looking for annual data or quarter data. 

Second, enter the ticker symbols of the companies you'd like to scrape for in the green cells highlighted in column 1.

Third, in the first three rows of the Google Sheet where the cells are highlighted green you should enter the statement, time period, and the account for each piece of data you would like returned. Here are valid entries for the 'statement' field: 'income-statement', 'balance-sheet', 'cash-flows'. 

Things get a little confusing for the Time Period row because there is a difference depending on whether you enter 'Annually' or 'Quarterly' into cell A1. If you're looking for annual data then enter the year you would like to scrape for (e.g. 2019). For quarterly data it gets tricky because different companies are on different schedules as far as their quarterly reporting goes. With that in mind, I made it so that the Time Period that is entered is in relation to the most recent quarter (e.g. Most Recent Quarter + 1). For the full key, click [here](supplementary_files/quarter_time_period.md). 

Lastly, the account that is entered should be an account that is listed on the web page for the given financial statement. Fortunately, for companies operating in the same industry, they will likely all have the same account names.

Now that I have gone over the set up, I will show you one example of how this code could be used for financial analysis. After setting up the Google Sheet like I just laid out, I would run the code and after it's done running the red cells would be populated. I then took this data and came up with the following table. 

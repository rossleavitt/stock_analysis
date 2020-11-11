# stock_analysis
Summary: This GitHub consists of 4 programs which scrape different websites for various kinds of data pertaining to a stock or a group of stocks. All this data was then used in a factor analysis of the Utility Industry. If you would like more information about the factor analysis that was done, visit here (LINK HERE 1). 

Below are short summaries for each of the programs that I made. After the summaries, I will go into more depth about each program, why I made it, and how I used it. 

Scrape_wsj: scrapes financial statement information (either from the balance sheet, income statement, or statement of cash flows) from wsj.com and enters that data into Google Sheets. Can specify between Quarterly and Yearly financial data. 

Scrape_yahoo: scrapes a company's Yahoo Financial Statistics page which offers information regarding that company's valuation ratios, profitability metrics, and other data for that company. 

Scrape_finra: scrapes Morningstar's Finra site so that I can have access to the bond yields of a particular bond offering from a company of interest. It will keep scraping until it reaches a certain predetermined date, giving the user time-series data of bond yields. 

Price_change: uses an API (documentation here (LINK HERE 2)) which returns time-series data of a stock's market price at close.



Scrape_wsj motivation and usage:

The motivation behind creating 'Scrape_wsj' was so that I could have a way of accessing many different companies' financial statement data in a timely manner. I could then use this data to compare similar companies on their use of debt, capital expenditures, cash flow from operations, etc. Below is what I came up with as far as telling the program what kind of information I want and where to put that information when it's done scraping. 
[xyz](docs/github1.PNG)

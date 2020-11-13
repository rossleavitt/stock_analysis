# stock_analysis
**Summary of stock_analysis:** This GitHub consists of 4 programs which scrape different websites for various kinds of data pertaining to a stock or a group of stocks. All this data was then used in a factor analysis of the Utility Industry. If you would like more information about the factor analysis that was done, visit here (LINK HERE 1). 

Below are short summaries for each of the programs that I made. Each program name links to that program's folder which contains the code for the program and a readme which goes into more information about each program, why I made it, and how I used it. 

**Scrape_wsj:** scrapes financial statement information (either from the balance sheet, income statement, or statement of cash flows) from wsj.com and enters that data into Google Sheets. Can specify between Quarterly and Yearly financial data. 

**Scrape_yahoo:** scrapes a company's Yahoo Financial Statistics page which offers information regarding that company's valuation ratios, profitability metrics, and other data for that company. 

**Scrape_finra:** scrapes Morningstar's Finra site so that I can have access to the bond yields of a particular bond offering from a company of interest. It will keep scraping until it reaches a certain predetermined date, giving the user time-series data of bond yields. 

**Price_change:** uses an API (documentation here (LINK HERE 2)) which returns time-series data of a stock's market price at close.

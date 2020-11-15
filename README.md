# stock_analysis
**Summary of stock_analysis:** This GitHub consists of four programs which scrape different websites for various kinds of data pertaining to a stock or a group of stocks. All this data was then used in a factor analysis of the Utility Industry. If you would like more information about the factor analysis that was done, visit my blog here (LINK HERE 1). 

I decided to make this one project instead of four separate ones because they all have the same end-goal: to allow the user to return financial data for an array of companies in a timely manner. Each program goes about that task differently since each web page is set up differently. Also, each one had an integral part in the factor analysis I did so it didn't make sense to me to split them up.

Below are short summaries for each of the programs that I made. Each program has a folder which contains the code for the program and a readme which goes into more information about each program, why I made it, and how I used it. 

3 of the 4 programs below utilize the Google Sheets API. A tutorial on how to set up the Google Sheets API in Python can be found [here](https://www.youtube.com/watch?v=cnPlKLEGR7E&t=3s&ab_channel=TechWithTim).

**Scrape_wsj:** scrapes financial statement information (either from the balance sheet, income statement, or statement of cash flows) from wsj.com and enters that data into Google Sheets. Can specify between Quarterly and Yearly financial data. 

**Scrape_yahoo:** scrapes a company's Yahoo Financial Statistics page which offers information regarding that company's valuation ratios, profitability metrics, and other general information relating to that company. Inputs data into Google Sheets.

**Scrape_finra:** scrapes Morningstar's Finra site so that I can have access to the bond yields of a particular bond offering from a company of interest. It will keep scraping until it reaches a certain predetermined date, giving the user time-series data of bond yields. Returns bond yield information in an Excel spreadsheet.

**Price_change:** uses an API (documentation [here](https://aroussi.com/post/python-yahoo-finance)) which returns time-series data of a stock's market price at close. Enters data into Google Sheets. 

**Price_change motivation and usage**

This program references an API created by Ran Aroussi. Find documentation [here](https://aroussi.com/post/python-yahoo-finance).

I developed this code because I wanted a way of accessing historical price data for a particular stock and using the above API was the easiest way to do so. Below is a short example of what this code should return using the tickers TSLA, MSFT, JPM, and AAPL. The green cells are user-input cells and everything else is returned by the program. *Note: in this example I only use four tickers but my program allows for up to 100 tickers to be entered into the first row starting with cell D1. Column C is reserved for Dates. Column B and A reserved for interval, start, end.*

![price_img_1](https://github.com/rossleavitt/stock_analysis/blob/main/images/price_img_1.PNG)

The following are valid entries for the interval cell: 1m, 2m, 5m, 15m, 30m, 60m, 90m, 1h, 1d, 5d, 1wk, 1mo, 3mo. Intraday historical price data only lasts for the last 60 days but I have found that I only really use the 1d and the 1mo interval.

The Start Date should be the day you want to begin tracking for. End Date should be the day after the last day you want to track for since End Date is not inclusive.

After that, run the code and the relevant cells will populate.

The main use that I had for this program was so that I could compare different companies within the same industry to see how each has fared over time. For example, [I used the 1mo interval to return data for all large-cap utility companies over the last 10 months or so](https://editor.wix.com/html/editor/web/renderer/edit/ab7b9e09-dee0-4a9b-b177-3180cbbefc95?metaSiteId=908cab2d-07f1-4376-8927-f5ee8b272d0e&editorSessionId=6cbdb0d9-363b-469c-b9ea-5e0e9a9f8e69&referralInfo=dashboard). Then I compared each company's increase or decrease in market price from 'pre-Covid to Covid', from 'Covid to now', and from 'pre-Covid to now'. This gave me a sense of the winners and losers so far due to the Covid-19 pandemic. 


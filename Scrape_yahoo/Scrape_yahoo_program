import bs4 as bs
import requests
import gspread
from oauth2client.service_account import ServiceAccountCredentials
import string
import time

#CONNECT TO GOOGLE SHEETS

scope = ["https://spreadsheets.google.com/feeds",'https://www.googleapis.com/auth/spreadsheets',"https://www.googleapis.com/auth/drive.file","https://www.googleapis.com/auth/drive"]

creds = ServiceAccountCredentials.from_json_keyfile_name("creds.json", scope)

client = gspread.authorize(creds)

workbook = client.open_by_url('https://docs.google.com/spreadsheets/d/1j-70U9OT4ZMLUF-VQjhEiXTaCi8z5w0VGOnHahutcfY/edit#gid=2024795337')

sheet = workbook.worksheet('Fin Test')


#creates arrays of data inputted on G Sheet in Green fields
tickers = sheet.col_values(1)[3:100]
type_array = sheet.row_values(1)[2:100]
labels = sheet.row_values(2)[2:100]
quarters = sheet.row_values(3)[2:100]


#label is the data we are looking for (e.g. 'Trailing Dividend' or 'Trailing P/E')
#quarter_num is the quarter that we would like to get the data for. Entered as an integer with 1 being the most recent
def valuation_measures(label, quarter_num):

    #sets up beautiful soup and gets the data set that we will be scraping in.
    #we are scraping for the data under "Valuation Measures" on the web page
    page = requests.get(url)
    soup = bs.BeautifulSoup(page.text, 'html.parser')
    valuation_data_table = soup.find("div", attrs={"class": "M(0) Whs(n) BdEnd Bdc($seperatorColor) D(itb)"})

    try:
        #finds all tr's under the valuation_data_table
        for table in valuation_data_table:
            trs = table.find_all("tr")

            #for each tr, find all the td's under it
            for tr in trs:
                tds = tr.find_all("td")

                #for each td, see if our label matches with the label on the web page
                for td in tds:
                    if label.lower() in td.text.lower():

                        #if there's a match, return the value that corresponds to that label and that 'quarter_num'
                        return tds[quarter_num].text

    #if something stops code from completing, return below
    except:
        return "Valuation Measures failed to complete"


#label is the data we are looking for (e.g. 'Trailing Dividend' or 'Trailing P/E')
def financial_stats(label):

    # sets up beautiful soup. Here we are pulling all the data under "Financial Highlights" and "Trading Information" on the web page
    page = requests.get(url)
    soup = bs.BeautifulSoup(page.text, 'html.parser')
    financial_stats_table = soup.find("div", attrs={"class": "Mstart(a) Mend(a)"})

    try:
        #under financial_stats_table, find div
        for data in financial_stats_table:
            divs = data.find('div')

            #for each div in divs, find all tables
            for div in divs:
                tables = div.find_all('table')

                #for each table, find all tr
                for table in tables:
                    trs = table.find_all('tr')

                    #for each tr, find all td
                    for tr in trs:
                        tds = tr.find_all('td')

                        #if label is found in tds[0], then return tds[1]. I will only ever want to return tds[1]
                        if label.lower() in tds[0].text.lower():
                            return tds[1].text

    #if something stops code from completing, return below
    except:
        return "Financial Stats failed to complete"


#used below to input on google sheet
num_to_letter = dict(enumerate(string.ascii_uppercase,1))

for tick_num, ticker in enumerate(tickers):

    #col_count used to keep track of where on the G sheets page we'd like to enter
    col_count = 0

    url = 'https://finance.yahoo.com/quote/' + ticker + '/key-statistics?p=' + ticker

    #iterates through type, label, quarter
    for type,label,quarter in zip(type_array,labels,quarters):
        if type == 'Financial':
            value = financial_stats(label)

        elif type == 'Valuation':
            value = valuation_measures(label,int(quarter))
        else:
            value = "Enter either 'Financial' or 'Valuation' under 'Type'"

        #col_count keeps track of the column we are entering on. It gets reset to 0 after each ticker.
        #tick_num keeps track of the row we are entering on.
        sheet.update((str(num_to_letter[col_count + 3]) + str(4 + tick_num)), value)

        col_count += 1

        #yahoo doesn't allow us to make a certain number of requests in 100 seconds (I believe there's a 100 request threshold)
        #slows down program so no errors are returned
        time.sleep(1)

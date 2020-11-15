from selenium import webdriver
import os
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import gspread
from oauth2client.service_account import ServiceAccountCredentials
import string

#connect to google sheets
scope = ["https://spreadsheets.google.com/feeds",'https://www.googleapis.com/auth/spreadsheets',"https://www.googleapis.com/auth/drive.file","https://www.googleapis.com/auth/drive"]
creds = ServiceAccountCredentials.from_json_keyfile_name("creds.json", scope)
client = gspread.authorize(creds)
workbook = client.open_by_url('https://docs.google.com/spreadsheets/d/1j-70U9OT4ZMLUF-VQjhEiXTaCi8z5w0VGOnHahutcfY/edit#gid=377077651')
sheet = workbook.worksheet('QA Template')

#define path to chrome driver
chrome_driver = os.path.abspath('C:/Users/ross/Desktop/chromedriver.exe')
browser = webdriver.Chrome(chrome_driver)


#USER ENTERED DATA

#ratios, per share data, percentage data should be listed below.
# All other data is multiplied by the units which are listed on the web page (usually thousands of dollars or millions of dollars)
unique_labels = sheet.col_values(15)[6:100]

#contains all tickers on g sheet
tickers_array = sheet.col_values(1)[3:100]

#the length of 'years' and 'tickers' gets the dimensions of our table on the g sheet.
#This is used to figure out the dimensions of where the user wants the information inputted into the google sheet.
time_period_len = len(sheet.row_values(2)[2:100])
tickers_len = len(tickers_array)

#are we looking for yearly or quarterly data? (cell A1)
quart_or_ann = sheet.cell(1,1).value


#FUNCTIONS TO BE USED LATER IN PROGRAM

#opens web page, sets the max number of divs we'll have to search through below and clicks any tabs to open the entire web page
def web_info(fin_stmt, ticker):


    #gets url using the ticker and financial statement
    browser.get("https://www.wsj.com/market-data/quotes/" + str(ticker) + "/financials/annual/" + str(fin_stmt))


    #if cell A1 in google sheet == 'Quarterly' then click quarterly button on web page
    if quart_or_ann == 'Quarterly':

        #clicks
        WebDriverWait(browser, 60).until(EC.element_to_be_clickable((By.CSS_SELECTOR, '#cr_cashflow > div.mod_tools.right > ul > li:nth-child(1) > a'))).click()



    #depending on the fin_stmt, the number of divs that will need to be iterated through will be different

    #the balance sheet needs to make one extra click, the cash flow statement needs to make two extras
    if fin_stmt == 'income-statement':

        div_max = 3

    elif fin_stmt == 'balance-sheet':

        div_max = 4

        WebDriverWait(browser, 60).until(EC.element_to_be_clickable((By.CSS_SELECTOR, '#cr_cashflow > div:nth-child(3) > div.cr_cashflow_strap > h2'))).click()

    elif fin_stmt == 'cash-flow':

        div_max = 5

        WebDriverWait(browser, 60).until(EC.element_to_be_clickable((By.CSS_SELECTOR, '#cr_cashflow > div:nth-child(3) > div.cr_cashflow_strap > h2'))).click()

        WebDriverWait(browser, 120).until(EC.element_to_be_clickable((By.CSS_SELECTOR, '#cr_cashflow > div:nth-child(4) > div.cr_cashflow_strap > h2'))).click()


    return div_max


#takes time period information from the second green row in the G Sheets and assigns the column that we should be looking in
def find_column(time_period):

    #col_num for 'Quarterly' is hardcoded in, instead of iteratively looking for the quarter I want.
    if quart_or_ann == 'Quarterly':

        if time_period == 'Most Recent Quarter':
            col_num = 1
        elif time_period == 'Most Recent Quarter + 1':
            col_num = 2
        elif time_period == 'Most Recent Quarter + 2':
            col_num = 3
        elif time_period == 'Most Recent Quarter + 3':
            col_num = 4
        elif time_period == 'This Quarter Last Year':
            col_num = 5


    #if 'Yearly' then we iteratively look through the web page for the year that we want to get the col_num
    if quart_or_ann == 'Yearly':
        year_data = WebDriverWait(browser, 60).until(EC.presence_of_all_elements_located((By.CSS_SELECTOR,

                                                                                          'table.cr_dataTable thead tr th')))


        # for each year value, check if it is equal to 'year' and if it is remember the column it is in and stop searching through years
        for i, column in enumerate(year_data):

            if column.text == time_period:
                col_num = i
                break

    return col_num


#will take account and search for the row we want to get. Then takes the row and col_num to get the value we are looking for
def find_value(account, col_num):

    div = 2

    while div < div_max:

        # as long as the div_max is not reached, continue to scrape the accounts on the web page (e.g. Cash, Accounts Receivable, etc.)
        account_data = WebDriverWait(browser, 60).until(EC.presence_of_all_elements_located((By.CSS_SELECTOR,'#cr_cashflow div:nth-child(' + str(div) + ') div.cr_cashflow_table table tbody tr td[class]:nth-child(1)')))


        #row_num keeps track of the number of rows that have been iterated through, to be used to get 'value' on line 152
        #account name goes through all values in account_data and sees if it matches with account
        for row_num, account_name in enumerate(account_data):

            if account_name.text == account:

                value = browser.find_element_by_css_selector('#cr_cashflow div:nth-child(' + str(div) + ') div.cr_cashflow_table table tbody tr:nth-child(' + str(row_num + 1) + ') td[class]:nth-child(' + str(col_num + 1) + ')')

                #once the desired account is reached, use the row number, column number, current div to retrieve the desired value

                return value.text

        div += 1


#accounts that aren't specified in 'unique_labels' will be multiplied by the units listed on the web page (usually in thousands or millions of dollars)
def get_units():
    find_labels = browser.find_element_by_css_selector('#cr_cashflow > div.expanded > div > table > thead > tr > th.fiscalYr')

    units = str(find_labels.text)

    if 'Thousands' in units:
        unit_multiplier = 1000
    elif 'Millions' in units:
        unit_multiplier = 1000000


    return unit_multiplier


#RUNS PROGRAM

#information_array holds each column that has been entered on the google sheet (the green cells in rows 1 through 3)
#each column on the g sheet should include the financial statement, time period, and account
information_array = []

for i in range(3, 3 + time_period_len):

    information_array.append(sheet.col_values(i)[0:3])


#full_array is one array which holds each ticker_array

full_array = []

#tickers_array contains all tickers entered in column 1
for ticker in tickers_array:

    #ticker_array holds all values for each ticker and then gets added to 'full_array' at the end and 'ticker_array' is erased
    ticker_array = []

    #if we have the same ticker and same financial statement as the previous iteration then we can skip running web_info
    #created last_fin_stmt so that I can see whether the cur_fin_stmt = last_fin_stmt
    last_fin_stmt = []


    for info in information_array:

        #information_array contains all the information that we will need for each stock (the statement, time_period, account)

        #info iterables renamed for clarity
        cur_fin_stmt = info[0]
        time_period = info[1]
        account = info[2]

        #if the financial statement equals the last financial statement that was just used, we skip opening a new web page and search with the current one
        if last_fin_stmt == cur_fin_stmt:


            #try finding column, then the value. If either doesn't work, append to the array the error message and move on to the next set of info
            try:

                column = find_column(time_period)

            except:

                ticker_array.append("Find Column could not be completed!")

                continue

            try:

                value = find_value(account, column)

            except:

                ticker_array.append("Find Value could not be completed!")

                continue

            #if the account is in unique_labels then we don't need to do anything with units so it immediately gets appended to the ticker_array
            if account in unique_labels:
                ticker_array.append(value)
                continue

            #if first character of 'value' is a '(' its a sign that the number is negative. Parenthesis are stripped, value is multiplied by units, parentesis are added back
            if value[0] == '(':
                no_parenthesis = value.replace('(','').replace(')','')
                unit_val = get_units() * float(no_parenthesis.replace(',',''))
                add_parenthesis = '(' + str(unit_val) + ')'
                ticker_array.append(add_parenthesis)

            #if no parenthesis then it gets multiplied by the units and appended
            else:
                unit_val = get_units() * float(value.replace(',', ''))
                ticker_array.append(unit_val)


        #if last_fin_stmt != cur_fin_stmt then we must open a new web page using web_info
        else:

            # info iterables renamed for clarity
            cur_fin_stmt = info[0]
            time_period = info[1]
            account = info[2]

            try:

                div_max = web_info(cur_fin_stmt, ticker)

            except:

                ticker_array.append("Web Info could not be completed!")

                continue


            try:

                column = find_column(time_period)

            except:

                ticker_array.append("Find Column could not be completed!")

                continue

            try:

                value = find_value(account, column)

            except:

                ticker_array.append("Find Value could not be completed!")

                continue

            #now set last_fin_stmt so that we can use it for the next iteration if we are using the same financial statement
            #if it is moving on to a new ticker all together then last_fin_stmt will be erased regardless because a new ticker means we need a new web page
            last_fin_stmt = cur_fin_stmt

            #checks to see if in unique labels (as before)
            if account in unique_labels:
                ticker_array.append(value)
                continue

            #multiplies by units and strips parenthesis, if applicable (as before)
            if value[0] == '(':
                no_parenthesis = value.replace('(','').replace(')','')
                unit_val = get_units() * float(no_parenthesis.replace(',',''))
                add_parenthesis = '(' + str(unit_val) + ')'
                ticker_array.append(add_parenthesis)

            else:
                unit_val = get_units() * float(value.replace(',', ''))
                ticker_array.append(unit_val)



    #all information for one ticker is held in ticker_array and then appended to the larger full_array which is below.
    #Then ticker_array is reset to nothing and move on to next ticker.
    full_array.append(ticker_array)


#creates dictionary to convert number to letter (1 to A, 2 to B, etc.)

num_to_letter_dict = dict(enumerate(string.ascii_uppercase, 1))

#full_array is inputted into area of g sheet decided by dimension of 'years_count' and 'tickers_count'

sheet.update("C4:" + str(num_to_letter_dict[2 + time_period_len]) + str(3 + tickers_len), full_array)

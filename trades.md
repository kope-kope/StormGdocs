# Trade Orders
***

As a Partner you can place BUY or SELL Trades for investors. Trades are typically executed within the trade-window of the NSE which is between 10AM and 2:20PM GMT+01:00 on workdays (Monday to Friday) except on public holidays. Trade requests made outside this window are queued for the next available trade-window. For example, trades placed on a Saturday are automatically queued for the next workday.

#### BUY Trade

Partners can place a trade on behalf of investors. However, for an investor to place a trade, he needs to have his virtual cash account funded. The virtual cash account can be funded by making a funds transfer to the Virtual NUBAN of the investor.  To confirm the receipt of payment into the virtual cash account, you can [GET the balance](api.md#get-balance) of the investor.

Once the account is confirmed funded, the [BUY trade](api.md#create-transaction) request can be placed. Partners can check the status of the trade by fetching the [list of  trades](api.md#get-transactions-by-date)  and the [trade details](api.md#get-transaction-details) 

Upon a successful trade request the investor gets an email notification of the trade request and upon execution, the investor also get an email notification.

The investor [cash balance](api.md#get-balance) is  depleted and his [portfolio size](api.md#get-portfolio)  increased.

#### SELL Trade

In a similar order an investor can place [SELL trades](api.md#create-transaction) . This depletes the [investor portfolio](api.md#get-portfolio) and increases his [cash balance](api.md#get-balance) 

# Transactions

The investor's transaction history can be fetched via the APIs and the are also available on the back office console.




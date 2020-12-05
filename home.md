# Introduction
**Welcome to Dart Invest!** Learn all about Dart Invest APIs and how to integrate them with your application. Dart Invest provides securities and brokerage services using a technology-driven approach. Dart Invest allows Trade Partners access to brokerage services in the African market starting with the Nigerian Stock Exchange. With Dart Invest, Trade Partners such as Banks, Fintechs, and other App Owners can integrate easily to the APIs to: 

1. Create and Manage Investor Accounts

2. Place Trades (BUY & SELL) for Investors

3. Gain Access to Market Information

Dart Invest achieves this by leveraging a partnership with stock brokerage firms in the market.

# Getting started

To get started as a trade partner on Dart Invest, Sign up on [www.dart.invest](https://console.staging.storm.trium.ng/register) and provide all the valid KYC documentation. While you wait for the approval of your partnership sign-up, you can commence integration with the [test keys](api.md#before-you-start). Upon approval, you would be able to fetch live keys.

Dart Invest console also allows you to access your transaction metrics and performance via the dashboard, manage investors' back-office support requests, and much more.

# Investors' Account
With Dart Invest, you can create investor accounts. Investors created on dart invest are registered with the Nigerian CSCS, a registered brokerage firm, and have a virtual cash account created. During investor account creation, the partner is required to provide a valid means of ID, which can be fetched from the investor directly or provided on behalf of the investor by the partner. The sequence of investor  creation involves a two-step process of 

1. Calling the [Create Investor](api.md#create-investor) API  

2. [Uploading the KYC](api.md#upload-kyc) (Valid means of ID)

The investor details are validated at the back-office and upon approval, the investor status becomes active and the investor is notified of the account creation with the broker and CSCS. This also means that the investor is ready to trade. In addition to this, you can: 

1. [Update investor details](api.md#update-investor)

2. [List investors](api.md#list-investors)

3. [Fetch Investor Balance](api.md#fetch-investor39s-balance)

4. [Fetch Investor Portfolio](api.md#fetch-investor39s-portfolio)

# Market Information & Symbols
Just before your investors start trading, they would require market information to make a BUY or SELL trade decision. Dart Invest grants you access to critical trade and market information. These include:

1. [Top Gainers](api.md#top-gainers-information)

2. [Top Losers](api.md#top-losers-information)

3. [Real-time Market News](api.md#market-news)

4. [Trade Symbols](api.md#symbols-list)

5. [Price List](api.md#price-list)

Data and insights gotten from the Market information is used as direct and indirect inputs in placing trade requests.

# Trade Orders

As a Partner, you can place BUY or SELL Trades for investors. Trades are typically executed within the trade-window of the NSE which is between 10 AM and 2:20 PM GMT+01:00 on workdays (Monday to Friday) except on public holidays. Trade requests made outside this window are queued for the next available trade-window.  For example, trades placed on a Saturday are automatically queued for the next workday Monday.

### BUY Trade

Partners can place a trade on behalf of investors. However, for an investor to place a trade, the investor's virtual cash account has to be funded. The virtual cash account can be funded by making a funds transfer to the Virtual NUBAN of the investor.  To confirm the receipt of payment into the virtual cash account, you can [get the balance](api.md#fetch-investor39s-balance) of the investor.

Once the account is confirmed funded, the [BUY trade](api.md#create-transaction)  request can be placed. Partners can check the status of the trade by fetching the [list of  trades](api.md#list-transactions-by-date)  and the [trade details](api.md#fetch-transactions-by-transaction-reference) 

Upon a successful trade request, the investor gets an email notification of the trade request and upon execution, the investor also gets an email notification.

The investor [cash balance](api.md#fetch-investor39s-balance) is  depleted while the [portfolio size](api.md#fetch-investor39s-portfolio) is increased.

### SELL Trade

In a similar order an investor can place [SELL trades](api.md#create-transaction) . This depletes the [investor portfolio](api.md#fetch-investor39s-portfolio) while the [cash balance](api.md#fetch-investor39s-balance) is increased.

### Transactions

The investor's transaction history can be fetched via [APIs](api.md#Fetch-Transactions-by-Transaction-Reference) with the transaction reference or by [date](api.md#list-transactions-by-date) and are also available on the back office console.





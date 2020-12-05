# Getting Started
Learn the step by step guide to initiate a BUY or SELL trade for your investors successfully on the live environment 

> To simulate this on Test mode,you can skip steps two, three and four.

#### Step One - Create Investor
Make a secure call to the [Create Investor](README.md#create-investor) API with the investor's details. Once successful, the status of the investor is set to `Pending`. You can [fetch the investor's details](README.md#fetch-investor) after creation or view via the [console](https://console.staging.storm.trium.ng/).
#### Step Two - Upload KYC
Before your investors can start to place trades on the NSE, you need to send in their KYC documents by making a secure call to the [Upload KYC](README.md#upload-kyc), these documents will be verified and the investor status is set to `active` upon verification. These documents include:

1. A permissible means of Identification which can be any of the following:
    -  International Passport
    -  Driver’s License
	-  Voters Card
	-  National ID card
 
2.	A permissible document for proof of address (the address provided when creating the investor) which can be any of the following:
	- Record of home visit in the case of Non-Nigerians
    - Address confirmation by Electoral Officer
    - Recent Utility bill (Should be issued by the Government e.g. Electricity bill, Waste Disposal, Nitel,Land use charge reciept etc.)
    - Bank Statement or pass book containing current address	
    - Solicitors letter confirming property purchase or Search report issued by the Lands registry.
    - Tenancy agreement
    - Search report of Investor's residence signed by a senior officer of the Capital market operator.

3. Passport Photograph

4. A scanned copy of the investor's signature

These four documents should be sent in a single .pdf file

#### Step Three - Fetch Investor's Details

Once your investor is `Active`, you can [fetch](README.md#fetch-investor) its important details including:

1. **Virtual Account Number** - Investor will use this to fund their brokerage account, which will be used to execute a trade.
2. **CSCS Number** - You will need this when initiating a transaction for investors
3. **Clearing House Number** - You won't need this to execute a trade but it is important for your investors. You can learn more about the [CHN](https://www.cscs.ng/faqs/)

#### Step Four - Funding

Your investor can now fund its brokerage account by making a bank transfer to the Virtual Account Number provided.

> Bank Name: Coronation Merchant Bank

When transfer is successful, make a secure call to [Get Account Balance](README.md#fetch-investor39s-balance) to confirm payment into investor's cash balane.

#### Step Five - Initiate a BUY Trade

Just before you initiate a BUY trade, you have to make secure calls to the [Market Information](README.md#market-information) endpoints to get the symbols of the stocks, the price and other relevant information. Proceed to [Create Transaction](README.md#create-transaction), set the trade_action = `BUY` and  specified transaction reference, This transaction reference will be used to manage the transaction i.e [fetch](README.md#list-transactions-by-date) and [cancel](#README.md#cancel-transactions) transaction. On completion of your trade, you can view the transactions list to know the status

#### Step Six - Initiate a SELL Trade

After a BUY trade is successfully executed, the [investor's portfolio](README.md#fetch-investor39s-portfolio) increases with the stocks bought. To sell this, make a secure call to the [Create Transaction](README.md#create-transaction),  set the trade_action = `SELL` and  specified transaction reference, This transaction reference will be used to manage the transaction i.e [fetch](README.md#list-transactions-by-date) and [cancel](#README.md#cancel-transactions) transaction. On completion of your trade, you can view the transactions list to know the status.

> The stock market opens between 10AM and 2:20PM (WAT) on workdays, Monday though Friday, except public holidays. Trades placed outside the trade window would be queued for exectution in the next trade window
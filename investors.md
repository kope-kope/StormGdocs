# Investors Account
With Dart Invest, you can create investor accounts. Investors created on dart invest are registered with the Nigerian CSCS, a registered brokerage firm and have a virtual cash account created. During investor account creation, the partner is required to provide a valid means of ID, which can be fetched from the investor directly or provided on behalf the investor by partner. The sequence of investor creation is involves a two step process of 

#### 1. Create Investor
Make a secure call to the [Create Investor](api.md#create-investor) API with the investor's details. Once successful, the status of the investor is set to `Pending`. You can [fetch the investor's details](api.md#fetch-investor) after creation or view via the [console](https://console.staging.storm.trium.ng/).
#### 2. Upload KYC
Before investors can start to place trades , you need to send in their KYC documents by making a secure call to the [Upload KYC](api.md#upload-kyc).These documents include:

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

The investor details are validated at the back-office and upon approval, the investor status becomes active and investor is notified of the account creation with broker and the CSCS. The also means that the investor is ready to trade :)

In addition to the this, you can 

[Update investor details](api.md#update-investor)

[List Investors](api.md#list-investors)

[Fetch Investor's Balance ](api.md#fetch-investor39s-balance)

[Fetch Investor's Portfolio ](api.md#fetch-investor39s-portfolio)
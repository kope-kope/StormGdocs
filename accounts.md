# Accounts
***
With Dart Invest, you can create investor accounts. Investors created on dart invest are registered with the Nigerian CSCS, a registered brokerage firm, and have a virtual cash account created. During investor account creation, the partner is required to provide a valid means of ID, which can be fetched from the investor directly or provided on behalf of the investor by the partner. The sequence of investor  creation involves a two-step process of 

1. Calling the [Create Investor](api.md#create-investor) API  

2. [Uploading the KYC](api.md#upload-kyc) (Valid means of ID)

The investor details are validated at the back-office and upon approval, the investor status becomes active and the investor is notified of the account creation with the broker and CSCS. This also means that the investor is ready to trade. In addition to this, you can: 

1. [Update investor details](api.md#update-investor)

2. [List investors](api.md#list-investors)

3. [Fetch Investor Balance](api.md#fetch-investor39s-balance)

4. [Fetch Investor Portfolio](api.md#fetch-investor39s-portfolio)

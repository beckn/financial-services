# Personal Loan Workflow (Using Electronically Consented Data Sharing)

The following steps outline the flow of Personal Loan Application using beckn protocol

## Loan Product Discovery

### Searching for personal loan products

To discover a loan product on a beckn-enabled network, the BAP must fire a `search` request to the BG containing atleast one the following fields

1. Loan Category
2. Loan Product
3. Loan Amount requested

#### An example `search` request containing the name of a loan category is shown below

```
{
    "context": {
        "domain": "financial-services:0.2.0",
        "location": {
            "country": {
                "code": "IND"
            }
        },
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "message_id": "bb579fb8-cb82-4824-be12-fcbc405b6608",
        "action": "search",
        "timestamp": "2023-05-25T05:23:03.443Z",
        "version": "1.1.0",
        "bap_uri": "https://credit-protocol-network.becknprotocol.io/",
        "bap_id": "credit-protocol.becknprotocol.io",
        "ttl": "PT10M"
    },
    "message": {
        "intent": {
            "category": {
                "descriptor": {
                    "name": "Personal loan"
                }
            }
        }
    }
}
```
> **Note:** If the loan categories are standardized at a network level, then the `message.intent.category.descriptor.name` may be replaced with `message.intent.category.descriptor.code` containing the standardized code of the loan category.

#### Example `search` Request for a Loan Product based on Loan Product Name and Loan Amount

```
{
    "context": {
        "domain": "financial-services:0.2.0",
        "location": {
            "country": {
                "code": "IND"
            }
        },
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "message_id": "bb579fb8-cb82-4824-be12-fcbc405b6608",
        "action": "search",
        "timestamp": "2023-05-25T05:23:03.443Z",
        "version": "1.1.0",
        "bap_uri": "https://credit-protocol-network.becknprotocol.io/",
        "bap_id": "credit-protocol.becknprotocol.io",
        "ttl": "PT10M"
    },
    "message": {
        "intent": {
            "item": {
                "descriptor": {
                    "name": "Personal loan"
                },
                "price": {
                    "value": "200000",
                    "currency": "INR"
                }
            }
        }
    }
}
```

### Returning a catalog of loan products

#### An example `on_search` callback containing a catalog of loan products offered by a single lender

```
{
    "context": {
        "domain": "financial-services:0.2.0",
        "location": {
            "country": {
                "code": "IND"
            }
        },
        "version": "1.1.0",
        "action": "on_search",
        "bap_id": "credit-protocol.becknprotocol.io",
        "bap_uri": "https://credit-protocol-network.becknprotocol.io/",
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "message_id": "bb579fb8-cb82-4824-be12-fcbc405b6608",
        "ttl": "PT30M",
        "timestamp": "2023-05-25T05:23:03.443Z",
        "bpp_id": "bpp.credit.icicibank.io",
        "bpp_uri": "https://bpp.credit.icicibank.io"
    },
    "message": {
        "catalog": {
            "descriptor": {
                "name": "ICICI Bank"
            },
            "providers": [
                {
                    "id": "1",
                    "descriptor": {
                        "images": [
                            {
                                "url": "https://www.icicibank.com/content/dam/icicibank/india/assets/images/header/logo.png"
                            }
                        ],
                        "code": "ICICIBANK",
                        "name": "ICICI Bank",
                        "short_desc": "ICICI Bank Ltd",
                        "long_desc": "ICICI Bank Ltd, India."
                    },
                    "categories": [
                        {
                            "id": "101123",
                            "descriptor": {
                                "name": "Personal loan"
                            }
                        }
                    ],
                    "items": [
                        {
                            "id": "66b7b9bad166-4a3f-ada6-ca063dc9d321",
                            "descriptor": {
                                "name": "Personal Loan"
                            },
                            "tags": [
                                {
                                    "descriptor": {
                                        "name": "General Information"
                                    },
                                    "list": [
                                        {
                                            "descriptor": {
                                                "name": "Interest Rate",
                                                "short_desc": "Rate of Interest (p.a)"
                                            },
                                            "value": "12%"
                                        }
                                    ],
                                    "display": true
                                }
                            ],
                            "matched": true
                        },
                        {
                            "id": "c8e3968c-cd78-4e46-aa34-0d541e46bd73",
                            "descriptor": {
                                "name": "Car Loan"
                            },
                            "tags": [
                                {
                                    "descriptor": {
                                        "name": "General Information"
                                    },
                                    "list": [
                                        {
                                            "descriptor": {
                                                "name": "Interest Rate",
                                                "short_desc": "Rate of Interest (p.a)"
                                            },
                                            "value": "8.5%"
                                        }
                                    ],
                                    "display": true
                                }
                            ],
                            "matched": false
                        },
                        {
                            "id": "80414936-a06d-49ae-9475-f99448c77014",
                            "descriptor": {
                                "name": "Education Loan"
                            },
                            "tags": [
                                {
                                    "descriptor": {
                                        "name": "General Information"
                                    },
                                    "list": [
                                        {
                                            "descriptor": {
                                                "name": "Interest Rate",
                                                "short_desc": "Rate of Interest (p.a)"
                                            },
                                            "value": "13%"
                                        }
                                    ],
                                    "display": true
                                }
                            ],
                            "matched": false
                        }
                    ]
                }
            ]
        }
    }
}
```


> **Note:** In modern lending flows, these interactions happen at the BAP's backend resulting in a pre-populated interface on the borrower's UI. Borrower applications may collect a majority of the user's information during signup and perform a scan across the network containing various lender platforms and their loan products. These loan products may appear as a pre-sorted static list on the borrower's app. However, this flow is not a mandatory. consumer applications with simpler UI's may default to a simple text-based search resulting in a dynamic catalog of search results obtained asynchronously from multiple lending platforms on the network 


## Loan Application

### Selection of a Loan Product

The Buyer app collects the borrower's name, address, date of birth, PAN (Permanent Account Number), phone number, income, employment type, and company name. This information is then shared with the lenders.

### Consent based Data Sharing

Step 2: Consent Creation and Retrieval of bureau Data
The Buyer app shares the borrower's account aggregator ID with the lenders. The lenders use this ID to create a consent request to access the borrower's account statement. 
Post the consent request creation, the Buyer app redirects the borrower to the account aggregator's page. Here, the borrower can review and approve or reject the consents. 
After the borrower approves the consent, the lender can retrieve the borrower's bank account statement through the account aggregator. 
The lender performs a soft bureau data pull, obtaining relevant information from the credit bureau to assess the borrower's creditworthiness.

### 

Step 3: Loan Offers
Depending on the borrower's credit history and account statement, the lenders either return pre-approved offers to the borrower or do not provide offers based on their underwriting criteria. 
The borrower can then choose to proceed with one of the offers or request an offer with a lower amount. If the borrower selects a lower amount, the lenders will share a revised offer..

Step 4: KYC
The lender orchestrates the KYC process by sharing a KYC link with the borrower. The borrower is redirected to the KYC URL, where they enter their KYC document details and may optionally share a one-time password to complete the KYC.

Step 5: Repayment setup 
Upon successful completion of KYC, the lender requests the borrower's disbursement account details and asks them to set up repayment using eNACH, eMandate, or standing instructions.

Step 6: Loan agreement sign and disbursal
The lender shares the complete loan agreement with the borrower through the Buyer app. The borrower is requested to enter the OTP (One-Time Password) provided by the lender to complete the eSigning process. 
Alternatively, the lender can provide a link to the loan agreement, allowing the borrower to review the agreement and proceed with Aadhar eSign. 
After signing the agreement, the lender may request account monitoring access on the borrower's account. Finally, the lender disburses the loan amount.


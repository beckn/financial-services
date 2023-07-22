# 5. Implementation Guide for BPPs

This document contains the REQUIRED and RECOMMENDED standard functionality that must be implemented by any Financial Service Provider Platform a.k.a BPPs. 

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [RFC2119].

## 5.1 Discovery of Financial Services
The following recommendations need to be considered when implementing discovery functionality for a Financial Service BPP

- REQUIRED. The platform MUST implement the `search` endpoint as per specifications described in beckn protocol specification.
- REQUIRED. The plaform MUST return a catalog of financial service products on the `on_search` callback endpoint implemented by the BAP.
- REQUIRED. The platform MUST map its financial service products to the `Item` schema.
- REQUIRED. Any financial service provider-related information like name, logo, short description must be mapped to the `Provider.descriptor` schema
- RECOMMENDED. If the platform wants to group its products under a specific category, it must map each category to the `Category` schema
- RECOMMENDED. Any service fulfillment-related information MUST be mapped to the `Fulfillment` schema.

### Example
A search request for a mutual fund purchase may look like this

```
{
    "context": {
        "action": "search",
        "bap_id": "https://example-fsc-platform.com",
        "bap_uri": "https://api.example-fsc-platform.com/",
        "domain": "financial-services:0.2.0",
        "location": {
            "country": {
                "code": "IND"
            }
        },
        "message_id": "bb579fb8-cb82-4824-be12-fcbc405b6608",
        "timestamp": "2023-05-25T05:23:03.443Z",
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "version": "1.1.0",
    },
    "message": {
        "intent": {
            "category": {
                "descriptor": {
                    "name": "Mutual Funds",
                    "code": "mf"
                }
            }
        }
    }
}
```

An example catalog of mutual funds may look like this
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
        "bap_id": "mutual-fund-protocol.becknprotocol.io",
        "bap_uri": "https://mutual-fund-protocol-network.becknprotocol.io/",
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "message_id": "bb579fb8-cb82-4824-be12-fcbc405b6608",
        "ttl": "PT30M",
        "timestamp": "2023-05-25T05:23:03.443Z",
        "bpp_id": "bpp.mutual-fund.abcmutualfunds.io",
        "bpp_uri": "https://bpp.mutual-fund.abcmutualfunds.io"
    },
    "message": {
        "catalog": {
            "descriptor": {
                "name": "ABC Mutual Funds"
            },
            "providers": [
                {
                    "id": "MF001",
                    "descriptor": {
                        "images": [
                            {
                                "url": "https://www.abcmutualfunds.com/content/dam/abc/india/assets/images/header/logo.png"
                            }
                        ],
                        "code": "ABCMF",
                        "name": "ABC Mutual Funds",
                        "short_desc": "ABC Mutual Funds Ltd",
                        "long_desc": "ABC Mutual Funds Ltd, India."
                    },
                    "categories": [
                        {
                            "id": "CAT101",
                            "descriptor": {
                                "name": "Large Cap",
                                "long_desc": "This mutual fund aims to provide long-term capital growth by investing in large-cap equities."
                            }
                        },
                        {
                            "id": "CAT102",
                            "descriptor": {
                                "name": "Corporate Bonds",
                                "long_desc": "This mutual fund aims to generate income by investing in high-quality corporate bonds."
                            }
                        },
                        {
                            "id": "CAT103",
                            "descriptor": {
                                "name": "Balanced",
                                "long_desc": "This mutual fund aims to provide a balance between capital growth and income by investing in a mix of equities and bonds."
                            }
                        }
                    ],
                    "items": [
                        {
                            "id": "MFLC001",
                            "descriptor": {
                                "name": "ABC Large Cap Mutual Fund - SIP"
                            },
                            "category_ids": [
                                "CAT101"
                            ],
                            "rating": "4.5",
                            "price": {
                                "value": "500",
                                "currency": "INR"
                            },
                            "tags": [
                                {
                                    "descriptor": {
                                        "name": "Mutual Fund Information"
                                    },
                                    "list": [
                                        {
                                            "descriptor": {
                                                "name": "Type",
                                                "short_desc": "Fund Type"
                                            },
                                            "value": "Equity"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Manager",
                                                "short_desc": "Fund Manager"
                                            },
                                            "value": "Tata Securities"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Inception Date",
                                                "short_desc": "Fund Inception Date"
                                            },
                                            "value": "2020-01-15"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "AUM",
                                                "short_desc": "Fund AUM"
                                            },
                                            "value": "250,000,000"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Expense Ratio",
                                                "short_desc": "Fund Expense Ratio"
                                            },
                                            "value": "1.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 month returns",
                                                "short_desc": "Fund 1 month returns"
                                            },
                                            "value": "2.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 month returns",
                                                "short_desc": "Fund 3 month returns"
                                            },
                                            "value": "6%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "6 month returns",
                                                "short_desc": "Fund 5 month returns"
                                            },
                                            "value": "7%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 year returns",
                                                "short_desc": "Fund 1 year returns"
                                            },
                                            "value": "15.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 year returns",
                                                "short_desc": "Fund 3 year returns"
                                            },
                                            "value": "45%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "5 year returns",
                                                "short_desc": "Fund 5 year returns"
                                            },
                                            "value": "60%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "SIP minimum investment amount",
                                                "short_desc": "Fund SIP minimum investment amount"
                                            },
                                            "value": "500"
                                        }
                                    ],
                                    "display": true
                                }
                            ],
                            "matched": true,
                            "xinput": {
                                "form": {
                                    "mime_type": "text/html",
                                    "url": "https://6vs8xnx5i7.abcmf.co.in/mf/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                                    "submission_id": "1c46b168-83ae-46ad-ac8a-d3c05870252c"
                                },
                                "required": true
                            }
                        },
                        {
                            "id": "MFLC001",
                            "descriptor": {
                                "name": "ABC Large Cap Mutual Fund - Lumpsum"
                            },
                            "category_ids": [
                                "CAT101"
                            ],
                            "rating": "4.5",
                            "price": {
                                "value": "> 5000",
                                "currency": "INR",
                                "minimum_value": "5000"
                            },
                            "tags": [
                                {
                                    "descriptor": {
                                        "name": "Mutual Fund Information"
                                    },
                                    "list": [
                                        {
                                            "descriptor": {
                                                "name": "Type",
                                                "short_desc": "Fund Type"
                                            },
                                            "value": "Equity"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Manager",
                                                "short_desc": "Fund Manager"
                                            },
                                            "value": "Tata Securities"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Inception Date",
                                                "short_desc": "Fund Inception Date"
                                            },
                                            "value": "2020-01-15"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "AUM",
                                                "short_desc": "Fund AUM"
                                            },
                                            "value": "250,000,000"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Expense Ratio",
                                                "short_desc": "Fund Expense Ratio"
                                            },
                                            "value": "1.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 month returns",
                                                "short_desc": "Fund 1 month returns"
                                            },
                                            "value": "2.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 month returns",
                                                "short_desc": "Fund 3 month returns"
                                            },
                                            "value": "6%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "6 month returns",
                                                "short_desc": "Fund 5 month returns"
                                            },
                                            "value": "7%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 year returns",
                                                "short_desc": "Fund 1 year returns"
                                            },
                                            "value": "15.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 year returns",
                                                "short_desc": "Fund 3 year returns"
                                            },
                                            "value": "45%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "5 year returns",
                                                "short_desc": "Fund 5 year returns"
                                            },
                                            "value": "60%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "SIP minimum investment amount",
                                                "short_desc": "Fund SIP minimum investment amount"
                                            },
                                            "value": "500"
                                        }
                                    ],
                                    "display": true
                                }
                            ],
                            "matched": true,
                            "xinput": {
                                "form": {
                                    "mime_type": "text/html",
                                    "url": "https://6vs8xnx5i7.abcmf.co.in/mf/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                                    "submission_id": "1c46b168-83ae-46ad-ac8a-d3c05870252c"
                                },
                                "required": true
                            }
                        },
                        {
                            "id": "MFCB002",
                            "descriptor": {
                                "name": "ABC Corporate Bonds Mutual Fund"
                            },
                            "category_ids": [
                                "CAT102"
                            ],
                            "rating": "4.2",
                            "tags": [
                                {
                                    "descriptor": {
                                        "name": "Mutual Fund Information"
                                    },
                                    "list": [
                                        {
                                            "descriptor": {
                                                "name": "Type",
                                                "short_desc": "Fund Type"
                                            },
                                            "value": "Fixed Income"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Manager",
                                                "short_desc": "Fund Manager"
                                            },
                                            "value": "Jane Smith Securities"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Inception Date",
                                                "short_desc": "Fund Inception Date"
                                            },
                                            "value": "2015-08-15"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "AUM",
                                                "short_desc": "Fund AUM"
                                            },
                                            "value": "120,000,000"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Expense Ratio",
                                                "short_desc": "Fund Expense Ratio"
                                            },
                                            "value": "0.95%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 month returns",
                                                "short_desc": "Fund 1 month returns"
                                            },
                                            "value": "2.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 month returns",
                                                "short_desc": "Fund 3 month returns"
                                            },
                                            "value": "6%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "6 month returns",
                                                "short_desc": "Fund 5 month returns"
                                            },
                                            "value": "7%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 year returns",
                                                "short_desc": "Fund 1 year returns"
                                            },
                                            "value": "5.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 year returns",
                                                "short_desc": "Fund 3 year returns"
                                            },
                                            "value": "15.2%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "5 year returns",
                                                "short_desc": "Fund 5 year returns"
                                            },
                                            "value": "20%"
                                        }
                                    ],
                                    "display": true
                                }
                            ],
                            "matched": true,
                            "xinput": {
                                "form": {
                                    "mime_type": "text/html",
                                    "url": "https://6vs8xnx5i7.abcmf.co.in/mf/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                                    "submission_id": "1c46b168-83ae-46ad-ac8a-d3c05870252c"
                                },
                                "required": true
                            }
                        },
                        {
                            "id": "MFBF002",
                            "descriptor": {
                                "name": "ABC Balanced Mutual Fund"
                            },
                            "category_ids": [
                                "CAT103"
                            ],
                            "tags": [
                                {
                                    "descriptor": {
                                        "name": "Mutual Fund Information"
                                    },
                                    "list": [
                                        {
                                            "descriptor": {
                                                "name": "Type",
                                                "short_desc": "Fund Type"
                                            },
                                            "value": "Hybrid"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Manager",
                                                "short_desc": "Fund Manager"
                                            },
                                            "value": "Robert Johnson"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Rating",
                                                "short_desc": "Fund Rating"
                                            },
                                            "value": "4"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Inception Date",
                                                "short_desc": "Fund Inception Date"
                                            },
                                            "value": "2012-04-05"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "AUM",
                                                "short_desc": "Fund AUM"
                                            },
                                            "value": "80,000,000"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Expense Ratio",
                                                "short_desc": "Fund Expense Ratio"
                                            },
                                            "value": "1.1%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 month returns",
                                                "short_desc": "Fund 1 month returns"
                                            },
                                            "value": "2.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 month returns",
                                                "short_desc": "Fund 3 month returns"
                                            },
                                            "value": "6%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "6 month returns",
                                                "short_desc": "Fund 5 month returns"
                                            },
                                            "value": "7%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "1 year returns",
                                                "short_desc": "Fund 1 year returns"
                                            },
                                            "value": "8.25%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "3 year returns",
                                                "short_desc": "Fund 3 year returns"
                                            },
                                            "value": "25.2%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "5 year returns",
                                                "short_desc": "Fund 5 year returns"
                                            },
                                            "value": "30%"
                                        },
                                        {
                                            "descriptor": {
                                                "name": "Investment Options",
                                                "short_desc": "Fund Investment Options"
                                            },
                                            "value": "SIP, Lumpsum"
                                        }
                                    ],
                                    "display": true
                                }
                            ],
                            "matched": true,
                            "xinput": {
                                "form": {
                                    "mime_type": "text/html",
                                    "url": "https://6vs8xnx5i7.abcmf.co.in/mf/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                                    "submission_id": "1c46b168-83ae-46ad-ac8a-d3c05870252c"
                                },
                                "required": true
                            }
                        }
                    ]
                }
            ]
        }
    }
}
```

## 5.2 Application for Financial Services

## 5.3 Fulfillment of Financial Services

## 5.4 Post-fulfillment of Financial Services

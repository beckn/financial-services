# 5. Implementation Guide

This document contains the REQUIRED and RECOMMENDED standard functionality that must be implemented by any Financial Service Provider Platform a.k.a BPPs and Financial Service Consumer Platform a.k.a BAPs.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in _TODO_

## 5.1 Discovery of Financial Services

### 5.1.1 Recommendations for BPPs
The following recommendations need to be considered when implementing discovery functionality for a Financial Service BPP

- REQUIRED. The BPP MUST implement the `search` endpoint to receive an `Intent` object sent by BAPs
- REQUIRED. The BPP MUST return a catalog of financial service products on the `on_search` callback endpoint specified in the `context.bpp_uri` field of the `search` request body.
- REQUIRED. The BPP MUST map its financial service products to the `Item` schema.
- REQUIRED. Any financial service provider-related information like name, logo, short description must be mapped to the `Provider.descriptor` schema
- REQUIRED. Any form that must be filled before receiving a quotation must be mapped to the `XInput` schema
- REQUIRED. If the platform wants to group its products under a specific category, it must map each category to the `Category` schema
- REQUIRED. Any service fulfillment-related information MUST be mapped to the `Fulfillment` schema.
- REQUIRED. If the BPP does not want to respond to a search request, it MUST return a `ack.status` value equal to `NACK`
- RECOMMENDED. Upon receiving a `search` request, the BPP SHOULD return a catalog that best matches the intent. This can be done by indexing the catalog against the various probable paths in the `Intent` schema relevant to typical financial service use cases

### 5.1.2 Recommendations for BAPs
- REQUIRED. The BAP MUST call the `search` endpoint of the BG to discover multiple BPPs on a network
- REQUIRED. The BAP MUST implement the `on_search` endpoint to consume the `Catalog` objects containing Financial Service Products sent by BPPs.
- REQUIRED. The BAP MUST expect multiple catalogs sent by the respective Financial Service Providers on the network
- REQUIRED. The financial service products can be found in the `Catalog.providers[].items[]` array in the `on_search` request
- REQUIRED. If the `catalog.providers[].items[].xinput` object is present, then the BAP MUST redirect the user to, or natively render the form present on the link specified on the `items[].xinput.form.url` field.
- REQUIRED. If the `catalog.providers[].items[].xinput.required` field is set to `"true"` , then the BAP MUST NOT fire a `select`, `init` or `confirm` call until the form is submitted and a successful response is received
- RECOMMENDED. If the `catalog.providers[].items[].xinput.required` field is set to `"false"` , then the BAP SHOULD allow the user to skip filling the form


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
                        }
                    ]
                }
            ]
        }
    }
}
```

## 5.2 Application for Financial Services
This section provides recommendations for implementing the APIs related to applying for a financial service.

### 5.2.1 Recommendations for BPPs

#### 5.2.1.1 Selecting a financial service from the catalog
- REQUIRED. The BPP MUST implement the `select` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST check for a form submission at the URL specified on the `xinput.form.url` before acknowledging a `select` request.
- REQUIRED. If the financial service provider has successfully received the information submitted by the financial service consumer, the BPP must return an acknowledgement with `ack.status` set to `ACK` in response to the `select` request
- REQUIRED. If the financial service provider has returned a successful acknowledgement to a `select` request, it MUST send the offer encapsulated in an `Order` object

#### 5.2.1.2 Initializing an application for a financial service
- REQUIRED. The BPP MUST implement the `init` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 5.2.1.3 Confirming the application for the financial service
- REQUIRED. The BPP MUST implement the `confirm` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 5.2.2 Recommendations for BAPs

#### 5.2.2.1 Selecting a financial service from the catalog

#### 5.2.2.2 Initializing an application for a financial service

#### 5.2.2.3 Confirming the application for the financial service

### 5.2.3 Example Workflow

### 5.2.3 Example Requests

Below is an example of a `select` request
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
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "action": "select",
    "timestamp": "2023-05-25T05:23:03.443Z",
    "version": "1.1.0",
    "bap_uri": "https://mutual-fund-protocol-network.becknprotocol.io/",
    "bap_id": "mutual-fund-protocol.becknprotocol.io",
    "ttl": "PT10M",
    "bpp_id": "bpp.mutual-fund.abcmutualfunds.io",
    "bpp_uri": "https://bpp.mutual-fund.abcmutualfunds.io"
  },
  "message": {
    "order": {
      "provider": {
        "id": "MF001"
      },
      "items": [
        {
          "id": "MFLC001"
        }
      ],
      "xinput": {
        "form_response": {
          "status": true,
          "submission_id": "c844d5f4-29c3-4398-b594-8b4716ef5dbf"
        }
      }
    }
  }
}
```

Below is an example of an `on_select` callback
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
    "action": "on_select",
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
    "order": {
      "provider": {
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
        }
      },
      "items": [
        {
          "id": "MFLC001",
          "descriptor": {
            "name": "ABC Large Cap Mutual Fund"
          },
          "category_ids": ["CAT101"],
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
                  "value": "John Doe"
                },
                {
                  "descriptor": {
                    "name": "Rating",
                    "short_desc": "Fund Rating"
                  },
                  "value": "4.5"
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
                    "name": "NAV",
                    "short_desc": "NAV"
                  },
                  "value": "120 rupees"
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
                    "name": "Investment Options",
                    "short_desc": "Fund Investment Options"
                  },
                  "value": "SIP, Lumpsum"
                },
                {
                  "descriptor": {
                    "name": "SIP minimum investment amount",
                    "short_desc": "Fund SIP minimum investment amount"
                  },
                  "value": "500"
                },
                {
                  "descriptor": {
                    "name": "Lumpsum minimum investment amount",
                    "short_desc": "Fund lumpsum minimum investment amount"
                  },
                  "value": "5000"
                }
              ],
              "display": true
            }
          ],
          "matched": true,
          "xinput": {
            "form": {
              "mime_type": "text/html",
              "url": "https://6vs8xnx5i7.abcmf.co.in/mf-kyc/xinput/formid/a23f2fdfbbb8ac402bfd54f",
              "submission_id": "1c46b168-83ae-46ad-ac8a-d3c05870252c"
            },
            "required": true
          }
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "120"
        }
      }
    }
  }
}
```


Below is an example of a `init` request
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
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "action": "init",
    "timestamp": "2023-05-25T05:23:03.443Z",
    "version": "1.1.0",
    "bap_uri": "https://mutual-fund-protocol-network.becknprotocol.io/",
    "bap_id": "mutual-fund-protocol.becknprotocol.io",
    "ttl": "PT10M",
    "bpp_id": "bpp.mutual-fund.abcmutualfunds.io",
    "bpp_uri": "https://bpp.mutual-fund.abcmutualfunds.io"
  },
  "message": {
    "order": {
      "provider": {
        "id": "MF001"
      },
      "items": [
        {
          "id": "MFLC001"
        }
      ],
      "payments": [
        {
          "tags": [
            {
              "descriptor": {
                "name": "Investment Method"
              },
              "list": [
                {
                  "descriptor": {
                    "name": "Type"
                  },
                  "value": "SIP"
                },
                {
                  "descriptor": {
                    "name": "Amount"
                  },
                  "value": "500"
                },
                {
                  "descriptor": {
                    "name": "SIP Start Date"
                  },
                  "value": "2023-01-07"
                },
                {
                  "descriptor": {
                    "name": "SIP Frequency"
                  },
                  "value": "Monthly"
                },
                {
                  "descriptor": {
                    "name": "SIP End Date"
                  },
                  "value": "2026-01-07"
                }
              ]
            }
          ]
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "500"
        }
      },
      "xinput": {
        "form_response": {
          "status": true,
          "submission_id": "c844d5f4-29c3-4398-b594-8b4716ef5dbf"
        }
      }
    }
  }
}
```

Below is an example of an `on_init` callback
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
    "action": "on_init",
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
    "order": {
      "provider": {
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
        }
      },
      "items": [
        {
          "id": "MFLC001",
          "descriptor": {
            "name": "ABC Large Cap Mutual Fund"
          },
          "category_ids": ["CAT101"],
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
                  "value": "John Doe"
                },
                {
                  "descriptor": {
                    "name": "Rating",
                    "short_desc": "Fund Rating"
                  },
                  "value": "4.5"
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
                    "name": "Investment Options",
                    "short_desc": "Fund Investment Options"
                  },
                  "value": "SIP, Lumpsum"
                },
                {
                  "descriptor": {
                    "name": "SIP minimum investment amount",
                    "short_desc": "Fund SIP minimum investment amount"
                  },
                  "value": "500"
                },
                {
                  "descriptor": {
                    "name": "Lumpsum minimum investment amount",
                    "short_desc": "Fund lumpsum minimum investment amount"
                  },
                  "value": "5000"
                }
              ],
              "display": true
            }
          ],
          "matched": true
        }
      ],
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "John Doe"
            },
            "contact": {
              "phone": "+91-9999999999",
              "email": "john.doe@example.com"
            }
          },
          "state": {
            "descriptor": {
              "name": "Mutual Fund Order Initialized"
            }
          }
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "5000"
        },
        "tags": [
          {
            "descriptor": {
              "name": "NAV Details"
            },
            "list": [
              {
                "descriptor": {
                  "name": "Unit price"
                },
                "value": "120"
              },
              {
                "descriptor": {
                  "name": "Units allocated"
                },
                "value": "41.50"
              }
            ]
          }
        ]
      },
      "payments": [
        {
          "type": "ON-ORDER",
          "url": "https://payment.abcmutalfunds.in",
          "params": {
            "amount": "5000",
            "currency": "INR"
          },
          "status": "NOT-PAID",
          "time": {
            "range": {
              "start": "01-07-2023 00:00:00",
              "end": "30-07-2023 23:59:59"
            }
          }
        }
      ],
      "cancellation_terms": [
        {
          "fulfillment_state": {
            "descriptor": {
              "name": "Terms"
            }
          },
          "external_ref": {
            "mimetype": "text/html",
            "url": "https://abcmutalfunds.com/mf/tnc.html"
          }
        }
      ],
      "tags": [
        {
          "list": [
            {
              "descriptor": {
                "name": "Mutual Fund Agreement",
                "short_desc": "Click on this link to view and sign your Agreement"
              },
              "value": "https://abcmutualfunds.com/agreement?id=afyrq9fbH"
            }
          ]
        }
      ]
    }
  }
}
```

Below is an example of a `confirm` request
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
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "action": "init",
    "timestamp": "2023-05-25T05:23:03.443Z",
    "version": "1.1.0",
    "bap_uri": "https://mutual-fund-protocol-network.becknprotocol.io/",
    "bap_id": "mutual-fund-protocol.becknprotocol.io",
    "ttl": "PT10M",
    "bpp_id": "bpp.mutual-fund.abcmutualfunds.io",
    "bpp_uri": "https://bpp.mutual-fund.abcmutualfunds.io"
  },
  "message": {
    "order": {
      "provider": {
        "id": "MF001"
      },
      "items": [
        {
          "id": "MFLC001"
        }
      ],
      "fulfillments": [
        {
          "stops": [
            {
              "authorization": {
                "type": "OTP",
                "token": "535346"
              }
            }
          ]
        }
      ],
      "payments": [
        {
          "params": {
            "source_bank_code": "SBIN0001234",
            "source_bank_account_number": "1800002341"
          }
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "500"
        }
      }
    }
  }
}
```
Below is an example of an `on_confirm` callback
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
    "action": "on_init",
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
    "order": {
      "id": "66B7AEDF45",
      "provider": {
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
        }
      },
      "items": [
        {
          "id": "MFLC001",
          "descriptor": {
            "name": "ABC Large Cap Mutual Fund"
          },
          "category_ids": ["CAT101"],
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
                  "value": "John Doe"
                },
                {
                  "descriptor": {
                    "name": "Rating",
                    "short_desc": "Fund Rating"
                  },
                  "value": "4.5"
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
                    "name": "Investment Options",
                    "short_desc": "Fund Investment Options"
                  },
                  "value": "SIP, Lumpsum"
                },
                {
                  "descriptor": {
                    "name": "SIP minimum investment amount",
                    "short_desc": "Fund SIP minimum investment amount"
                  },
                  "value": "500"
                },
                {
                  "descriptor": {
                    "name": "Lumpsum minimum investment amount",
                    "short_desc": "Fund lumpsum minimum investment amount"
                  },
                  "value": "5000"
                }
              ],
              "display": true
            }
          ],
          "matched": true
        }
      ],
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "John Doe"
            },
            "contact": {
              "phone": "+91-9999999999",
              "email": "john.doe@example.com"
            }
          },
          "state": {
            "descriptor": {
              "name": "Mutual Fund order confirmed"
            }
          }
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "500"
        },
        "tags": [
          {
            "descriptor": {
              "name": "NAV Details"
            },
            "list": [
              {
                "descriptor": {
                  "name": "Unit price"
                },
                "value": "120"
              },
              {
                "descriptor": {
                  "name": "Units allocated"
                },
                "value": "4.15"
              }
            ]
          }
        ]
      },
      "payments": [
        {
          "type": "ON-FULFILLMENT",
          "url": "https://emandate.abcmutalfunds.in",
          "params": {
            "amount": "500",
            "currency": "INR"
          },
          "status": "PAID",
          "time": {
            "range": {
              "start": "01-07-2023 00:00:00",
              "end": "30-07-2023 23:59:59"
            }
          }
        }
      ],
      "cancellation_terms": [
        {
          "fulfillment_state": {
            "descriptor": {
              "name": "Terms"
            }
          },
          "external_ref": {
            "mimetype": "text/html",
            "url": "https://abcmutalfunds.com/mf/tnc.html"
          }
        }
      ]
    }
  }
}
```

## 5.3 Fulfillment of Financial Services
This section contains recommendations for implementing the APIs related to fulfilling a financial service

### 5.3.1 Recommendations for BPPs

#### 5.3.1.1 Sending status updates
- REQUIRED. The BPP MUST implement the `status` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 5.3.1.2 Updating an application for financial service
- REQUIRED. The BPP MUST implement the `update` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 5.3.1.3 Cancelling a financial service application
- REQUIRED. The BPP MUST implement the `cancel` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST implement the `get_cancellation_reasons` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 5.3.1.4 Real-time tracking
- REQUIRED. The BPP MUST implement the `track` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 5.3.2 Recommendations for BAPs

#### 5.3.2.1 Sending status updates

#### 5.3.2.2 Updating an application for financial service

#### 5.3.2.3 Cancelling a financial service application

#### 5.3.2.4 Real-time tracking

### 5.3.3 Example Workflow

### 5.3.4 Example Requests

Below is an example of a `status` request
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
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "action": "status",
    "timestamp": "2023-05-25T05:23:03.443Z",
    "version": "1.1.0",
    "bap_uri": "https://mutual-fund-protocol-network.becknprotocol.io/",
    "bap_id": "mutual-fund-protocol.becknprotocol.io",
    "ttl": "PT10M",
    "bpp_id": "bpp.mutual-fund.abcmutualfunds.io",
    "bpp_uri": "https://bpp.mutual-fund.abcmutualfunds.io"
  },
  "message": {
    "order_id": "66B7AEDF45"
  }
}
```

Below is an example of an `on_status` callback
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
    "action": "on_init",
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
    "order": {
      "id": "66B7AEDF45",
      "status": "PROCESSING",
      "provider": {
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
        }
      },
      "items": [
        {
          "id": "MFLC001",
          "descriptor": {
            "name": "ABC Large Cap Mutual Fund"
          },
          "category_ids": ["CAT101"],
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
                  "value": "John Doe"
                },
                {
                  "descriptor": {
                    "name": "Rating",
                    "short_desc": "Fund Rating"
                  },
                  "value": "4.5"
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
                    "name": "Investment Options",
                    "short_desc": "Fund Investment Options"
                  },
                  "value": "SIP, Lumpsum"
                },
                {
                  "descriptor": {
                    "name": "SIP minimum investment amount",
                    "short_desc": "Fund SIP minimum investment amount"
                  },
                  "value": "500"
                },
                {
                  "descriptor": {
                    "name": "Lumpsum minimum investment amount",
                    "short_desc": "Fund lumpsum minimum investment amount"
                  },
                  "value": "5000"
                }
              ],
              "display": true
            }
          ],
          "matched": true
        }
      ],
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "John Doe"
            },
            "contact": {
              "phone": "+91-9999999999",
              "email": "john.doe@example.com"
            }
          },
          "state": {
            "descriptor": {
              "name": "Units being allocated"
            }
          }
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "5000"
        },
        "tags": [
          {
            "descriptor": {
              "name": "NAV Details"
            },
            "list": [
              {
                "descriptor": {
                  "name": "Unit price"
                },
                "value": "120"
              },
              {
                "descriptor": {
                  "name": "Units allocated"
                },
                "value": "41.50"
              }
            ]
          }
        ]
      },
      "payments": [
        {
          "type": "ON-ORDER",
          "url": "https://payment.abcmutalfunds.in",
          "params": {
            "amount": "5000",
            "currency": "INR"
          },
          "status": "PAID",
          "time": {
            "range": {
              "start": "01-07-2023 00:00:00",
              "end": "30-07-2023 23:59:59"
            }
          }
        }
      ],
      "cancellation_terms": [
        {
          "fulfillment_state": {
            "descriptor": {
              "name": "Terms"
            }
          },
          "external_ref": {
            "mimetype": "text/html",
            "url": "https://abcmutalfunds.com/mf/tnc.html"
          }
        }
      ]
    }
  }
}
```

Below is an example of a `update` request
> This section is currently under development

Below is an example of an `on_update` callback
> This section is currently under development

Below is an example of a `cancel` request
> This section is currently under development

Below is an example of an `on_cancel` callback
> This section is currently under development

Below is an example of a `track` request
> This section is currently under development

Below is an example of an `on_track` callback
> This section is currently under development

## 5.4 Post-fulfillment of Financial Services
This section contains recommendations for implementing the APIs after fulfilling a financial service

### 5.4.1 Recommendations for BPPs

#### 5.4.1.1 Rating and Feedback
- REQUIRED. The BPP MUST implement the `rating` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST implement the `get_rating_categories` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 5.4.1.2 Providing Support
- REQUIRED. The BPP MUST implement the `support` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 5.4.2 Recommendations for BAPs

#### 5.4.2.1 Rating and Feedback

#### 5.4.2.2 Providing Support

### 5.4.3 Example Workflow

### 5.4.4 Example Requests

Below is an example of a `get_rating_categories` request

Below is an example of an `rating_categories` callback

Below is an example of a `rating` request

Below is an example of an `on_rating` callback

Below is an example of a `support` request
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
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "action": "support",
    "timestamp": "2023-05-25T05:23:03.443Z",
    "version": "1.1.0",
    "bap_uri": "https://mutual-fund-protocol-network.becknprotocol.io/",
    "bap_id": "mutual-fund-protocol.becknprotocol.io",
    "ttl": "PT10M",
    "bpp_id": "bpp.mutual-fund.abcmutualfunds.io",
    "bpp_uri": "https://bpp.mutual-fund.abcmutualfunds.io"
  },
  "message": {
    "support": {
      "order_id": "66b7b9bad166",
      "phone": "+919876543210",
      "email": "john.doe@gmail.com"
    }
  }
}
```

Below is an example of an `on_support` callback
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
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "action": "on_support",
    "timestamp": "2023-05-25T05:23:03.443Z",
    "version": "1.1.0",
    "bap_uri": "https://mutual-fund-protocol-network.becknprotocol.io/",
    "bap_id": "mutual-fund-protocol.becknprotocol.io",
    "ttl": "PT10M",
    "bpp_id": "bpp.mutual-fund.abcmutualfunds.io",
    "bpp_uri": "https://bpp.mutual-fund.abcmutualfunds.io"
  },
  "message": {
    "support": {
      "order_id": "66b7b9bad166",
      "phone": "1800 1080",
      "email": "customer.care@abcmutualfunds.com",
      "url": "https://www.abcmutualfunds.com/helpdesk"
    }
  }
}
```

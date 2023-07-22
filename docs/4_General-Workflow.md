# A General Workflow for Availing Financial Services

## Authors
1. Ravi Prakash - FIDE
2. Hrushikesh Mehta - ONDC
3. Antriksh Parmar - ONDC

## Overview

Any financial services application consists of the following flow. 

```mermaid
graph TD
    A[Start] --> B[Discovery of Financial Services];
    B[Discovery of Financial Services] --> C[Application for financial service];
    C[Application for financial service] --> D[Fulfillment of financial service];
    D[Fulfillment of financial service] --> E[Post-fulfillment of financial service];
    E[Post-fulfillment of financial service] --> F[Stop];
```

Each of these steps consist of one or more interactions that allow a financial service consumer (FSC) to avail services offered by a financial services provide (FSP). These financial services can be anything ranging from Insurance, Mutual Funds, Credit, or any other service. 

> Note: Bear in mind, this is just an example workflow for a simple transaction between a Consumer and a Provider. The order of interactions between the two parties is arbitratrily selected on the basis of several interviews with subject matter experts.

A typical workflow for a financial service transaction consits of the following steps.
1. Discovery of Financial Services
2. Application for a Financial Service
3. Fulfillment of the Financial Service
4. Post-fulfillment of Financial Services

## 1. Discovery of Financial Services

The logical workflow for the discovery of a financial service is shown below

```mermaid
sequenceDiagram
    Actor Financial Service Consumer
    participant Online & Offline Media
    participant Financial Service Provider
    loop Open Search
        Financial Service Consumer->> Online & Offline Media: Research financial service providers (FSPs)
        Online & Offline Media->>Financial Service Consumer: Return financial service providers
    end
    Financial Service Consumer->>Financial Service Provider: Express intent to avail financial service
    Financial Service Provider->>Financial Service Consumer: Provide catalog of financial services
    loop Browse Catalog
        Financial Service Consumer->>Financial Service Provider: View a desired financial service
        Financial Service Provider->>Financial Service Consumer: Display detailed information of the service
    end
```

Let us look at these steps in detail. 

### 1.1: FSC discovers an FSP 
The consumer researches various financial service providers through known  aggregators, search engines, broadcast media, personal connections etc. 

An FSC can search for an FSP in many ways like
- Searching for a FSPs, (say, a bank) by its name or code
- Searching for FSPs based on rating
- Searching for financial services by their names
- Searching for financial services by their category name
- Searching for financial services by amount

Eventually, the FSC discovers an FSP that matches their requirement.

### 1.2 FSC browses through various products of an FSP
In this step, the FSC discovers an FSP and browses through the various financial sevice products in its catalog.

In this interaction, the FSP publishes their catalog of products. A FSP can publish various types of catalogs like,
- Catalog of Financial Service Products
- Catalog of Financial Service Categories

### 1.3 FSC views a particular product of an FSP
In this step, the FSC discovers a desired financial service in the catalog and views its details. 

## 2. Application for a Financial Service
In this stage, the FSC selects a particular financial product and begins the process of applying for it. During the process of application, there are several sub-interactions that happen between the FSC and the FSP. 

### 2.1 FSC selects a service
In this step, the FSC requests an offer made by the FSP. In contrast to typical commercial transactions, in financial services, the application typically begins by providing the FSP with some information. This information is provided via a form submission containing all the relevant information. Sometimes, the provider might also request the consumer to share financial records like  transaction records, invoicing history, or any other information that belongs to the FSC. In some modern financial service applications, the fetching of information can also happen via an electronic consent request made by the FSP to the FSC. Once the consent is given by the consumer, the FSP pulls the financial records through the consent infrastructure.

### 2.2 FSP processes the financial information and returns an offer
Once the financial information is fetched, the FSP processes the financial information provided by the FSC and returns an offer. This offer may not necessarily be the final offer, but more like an indication of what the FSC is eligible for. Receipt of an offer is not a guarantee of the final offer made by the FSP to the FSC.

### 2.3 FSC accepts the offer
In this step, the FSC accepts the offer. Sometimes, the FSP requests for additional information from the FSC in order to finalize the application. This too can happen via a simple form submission or via a consent request, or both. 

### 2.4 FSP generates the final agreement
In this step, the FSP generates the final agreement with the final offer, the terms of service.

### 2.5 FSC signs the agreement
In this step, the FSC signs the agreement and confirms the application.

### 2.6 FSP confirms the application
In this step, the FSP returns the finalized application with the latest status.

## 3. Fulfillment of the Financial Service
In this stage, the application is confirmed and the FSP initiates the fulfillment of the financial service. In the case of credit, it could mean the actual disbursal of the loan. In the case of insurance, it could mean claiming an insurance, or a request for a premium to be deposited. In the case of mutual funds, it could be a request for a SIP payment. Furthermore, in this stage, the FSC can also cancel the service due to various reasons. Sometimes, the FSP can also update the terms of service based on events at the FSP or the FSC's end like an advance payment of the loan. Update of bank account, etc. As always, the Fulfillment stage of a financial service can be composed of further sub-interactions. Let us look at the types of interactions possible at this stage. 

### 3.1 Status Updates

### 3.2 Terms Update

### 3.3 Cancellation of Service

### 3.4 Tracking

## 4. Post-fulfillment of Financial Services
In this stage, all the events that happen after the financial service is fulfilled can happen. For example, rating a service, contacting support, settlements, and defaults. 

### 4.1 Rating a service

### 4.2 Contacting Support

#### Step 3: Lender reviews the loan application
The lender reviews the loan application submitted by the borrower.
They assess the borrower's eligibility based on factors such as credit score, income stability, employment history, and existing financial obligations.

#### Step 4: Lender requests additional documentation
If the initial application meets the lender's requirements, they may request additional documentation to verify the borrower's information.
Commonly requested documents include identity proof, address proof, income proof (salary slips, bank statements, income tax returns), employment proof, and any other specific documents mentioned by the lender.

#### Step 5: Lender evaluates the documents
The lender examines the submitted documents to verify the borrower's details.
They verify the authenticity of the documents and cross-check the information provided in the application form.

#### Step 6: Lender assesses the creditworthiness
Based on the borrower's credit history, income, and other relevant factors, the lender determines the borrower's creditworthiness.
They evaluate the borrower's ability to repay the loan by considering their debt-to-income ratio, credit score, and other financial aspects.

#### Step 7: Lender approves or rejects the loan application
After assessing the borrower's creditworthiness, the lender makes a decision on approving or rejecting the loan application.
If approved, the lender communicates the loan amount, interest rate, tenure, and other terms and conditions to the borrower.

#### Step 8: Borrower accepts the loan offer
If the loan application is approved, the borrower reviews the loan offer provided by the lender.
They carefully read and understand the terms and conditions, interest rate, repayment schedule, and any associated fees or charges.
If satisfied, the borrower accepts the loan offer by signing the loan agreement.

#### Step 9: Lender disburses the loan amount
Upon receiving the borrower's acceptance, the lender initiates the loan disbursal process.
They transfer the approved loan amount to the borrower's designated bank account.

#### Step 10: Borrower starts repaying the loan
Once the loan amount is disbursed, the borrower starts making repayments as per the agreed-upon .
They can typically repay the loan through equated monthly instalments (EMIs) via post-dated checks, automatic bank transfers, or online payment methods.

#### Step 11: Lender monitors loan repayments
The lender keeps track of the borrower's repayments throughout the loan tenure.
They send periodic statements showing the loan balance, EMI due dates, and any other relevant information.

#### Step 12: Borrower completes loan repayment
The borrower continues repaying the loan as per the agreed schedule until the entire loan amount, along with the applicable interest, is repaid.
Once the loan is fully repaid, the borrower receives a loan closure confirmation from the lender.

According to beckn protocol, any consumer-provider interaction can be broken down into four stages namely, Discovery, Ordering, Fulfillment and Post-Fulfillment. Without loss of generality, let us break the above credit application process into the four stages and describe the subinteractions that occur within them.


### Beckn Protocol API Workflow
In beckn protocol, the search intent generated by the Borrower Platform (BAP) is typically published on the gateway (BG) that broadcasts the intent to multiple lender platforms (BPPs). Each of the BPPs return their catalogs directly to the BAP via asynchronous callbacks. The workflow for that is shown below.
```mermaid
    sequenceDiagram
    Borrower Platform (BAP)->>Gateway (BG): Declare Intent to Apply for Credit ( search )
    Gateway (BG)->>Registry: Lookup Lender Platforms (lookup)
    Registry->>Gateway (BG): List of Lender Platforms (200 OK)
    Gateway (BG)->>Lender Platform 1 (BPP1): Declare Intent to Apply for Credit (search)
    Gateway (BG)->>Lender Platform 2 (BPP2): Declare Intent to Apply for Credit (search)
    Gateway (BG)->>Lender Platform n (BPPn): Declare Intent to Apply for Credit (search)
    Lender Platform 1 (BPP1)->>Borrower Platform  (BAP): Publish Catalog of Lender 1 (on_search)
    Lender Platform 2 (BPP2)->>Borrower Platform  (BAP): Publish Catalog of Lender 2 (on_search)
    Lender Platform n (BPPn)->>Borrower Platform  (BAP): Publish Catalog of Lender n (on_search)
```

## Ordering (Loan Application Stage)
Applying for a loan consists of multiple interactions like,

### Borrower-side actions
1. Selecting lender(s)
2. Submitting a Know your Customer (KYC) form
3. Sharing financial information like Bank Statements, Invoicing Data, etc
4. Signing the Loan Agreement

### Lender-side actions
1. Requesting for financial information
2. Requesting KYC details
3. Making a loan offer
4. Creating the loan agreement

### Logical Workflow

The below diagram illustrates the logical interactions between a borrower and a lender during the Loan Application stage. 

```mermaid
    sequenceDiagram
    Borrower->>Lender: Borrower selects loan product
    Lender->>Borrower: Requests for financial information
    Borrower->>Lender: Borrower submits financial information
    Lender->>Borrower: Lender makes an Offer
    Borrower->>Lender: Borrower agrees to offer
    Lender->>Borrower: Lender generates loan agreement and requests for KYC details
    Borrower->>Lender: Borrower provides KYC details and signs loan agreement
    Lender->>Borrower: Lender confirms loan application    
```
### Beckn Protocol API Workflow

```mermaid
    sequenceDiagram
    Borrower Platform (BAP)->>Lender Platform 1 (BPP1): Select loan product from lender's catalog (select)
    Lender Platform 1 (BPP1)->>Borrower Platform (BAP): Send loan product details with a request to share financial information (on_select)
    Borrower Platform (BAP)->>Lender Platform 1 (BPP1): Submits financial information (init)
    Lender Platform 1 (BPP1)->>Borrower Platform (BAP): Send loan offer with EMI details and link to loan agreement form (on_init)
    Borrower Platform (BAP)->>Lender Platform 1 (BPP1): Agrees to loan offer (confirm)
    Lender Platform 1 (BPP1)->>Borrower Platform (BAP): Send confirmed loan application with disbursal status (on_confirm)
```

## Fulfillment (Sanction and Disbursal Stage)

In this stage, the loan application has been submitted successfully. During this stage, the borrower or the lender can perform actions like

1. Status updates regarding confirmation, disbursal, repayment etc
2. Termination of a loan application
3. Updating the terms of a loan agreement
4. Requesting additional information

These actions can be initiated at the borrower's end as well as the lender's end.

### Borrower side actions
The borrower can perform the following actions after a loan has been applied

1. Requesting for a status update on the loan application
2. Request to terminate a loan agreement
3. Request to update specific details in the loan application

### Lender-side actions

1. Providing updates on the status of the loan application, disbursal, repayment etc
2. Terminating a loan agreement
3. Updating the terms of a loan agreement or other details

### Logical Workflow

#### Request for status update (with unsolicited status updates from lender's side)

```mermaid
    sequenceDiagram
    Borrower->>Lender: Request loan application status (status)
    Lender->>Borrower: Provide loan application status (on_status)
    Lender->>Borrower: Provide loan application status [1] (on_status)
    Lender->>Borrower: Provide loan application status [2] (on_status)
    Lender->>Borrower: Provide loan application status [3] (on_status)
```

#### Requesting termination of a loan agreement

```mermaid
    sequenceDiagram
    Borrower->>Lender: Request to terminate loan agreement (cancel)
    Lender->>Borrower: Send terminated loan agreement terms (on_cancel)
```

#### Unsolicited trmination of a loan agreement from lender's side

```mermaid
    sequenceDiagram
    Lender->>Borrower: Send terminated loan agreement terms (on_cancel)
```
> **Note**: The lender and the borrower boxes are exchanged due to limitations in the mermaid UML plugin. This limitation only allows sequence diagrams to start from left to right. 

#### Requesting update of a loan agreement

```mermaid
    sequenceDiagram
    Borrower->>Lender: Request to update loan agreement (update)
    Lender->>Borrower: Send updated loan agreement with terms (on_update)
```

#### Uncolicited update of a loan agreement from the lender's side

```mermaid
    sequenceDiagram
    Lender->>Borrower: Send updated loan agreement with terms (on_update)
```

> **Note**: The lender and the borrower boxes are exchanged due to limitations in the mermaid UML plugin. This limitation only allows sequence diagrams to start from left to right. 

## Post-Fulfillment (Repayment Stage)

This stage contains everything that happens after the sanction and disbursal of a loan like,

1. Repayment of loans
2. Defaults
3. Rating
4. Support

### Repayment of the Loan

After the loan is sanctioned and funds are disbursed, the borrower begins the loan repayment phase. This involves making regular payments (EMI) according to the agreed-upon repayment schedule outlined in the loan agreement. The borrower typically repays the loan through monthly instalments that include both the principal amount and the interest. This repayment is done via existing payment infrastrucutures like Netbanking, Payment Gateways, Cheque Deposits, and in some supported regions like India - UPI.

Beckn protocol does not support payment transactions in its API and relies on existing payment infrastructures to facilitate payments. However, the terms of repayment like the EMI amount, frequency of EMI, bank account details etc are exchanged via beckn protocol APIs. Once an EMI payment is made, it is recommended that the lender send a status update regarding the receipt of payment and an updated order containing the balance amount.


### Pre-payment of a Loan
Sometimes the borrower pre-pays a loan or its EMI before its due date. The typical workflow is illustrated here.

#### Step 1: Notify the Lender
The borrower notifies the lender of their intention to make a prepayment.
This notification can be made through various channels such as phone call, email, or in-person visit, as specified by the lender.

#### Step 2: Obtain Prepayment Quotation
The lender provides the borrower with a prepayment quotation.
The quotation includes the exact amount to be paid for prepayment, considering any applicable fees or charges.

#### Step 3: Initiate Prepayment
The borrower initiates the prepayment process by transferring the agreed-upon amount to the lender.
This transfer can be made through electronic funds transfer, online payment, or any other method specified by the lender.

#### Step 4: Update Loan Account
The lender updates the borrower's loan account to reflect the prepayment.
They record the prepayment amount and adjust the outstanding loan balance accordingly.

#### Step 5: Provide Confirmation
The lender provides confirmation to the borrower regarding the successful prepayment.

#### Step 6: Update Repayment Schedule
If applicable, the lender updates the borrower's repayment schedule based on the prepayment made.
They adjust the remaining EMIs (Equated Monthly Instalments) or revise the loan tenure, considering the reduced outstanding balance.

The illustrated diagram of the above workflow is shown here

```mermaid
    sequenceDiagram
    Borrower->>Lender: Notify Intention to Prepay
    Lender->>Borrower: Provide Prepayment Quotation
    Borrower->>Lender: Initiate Prepayment
    Lender->>Lender: Update Loan Account
    Lender->>Borrower: Provide Confirmation
    Lender->>Lender: Update Repayment Schedule
```

### Loan Defaults

The below workflow is usually initiated when the borrower fails to make the scheduled repayment. Bear in mind, this is just an example workflow and can change basis the loan type, region, and amount. 

### Step 1: Missed Payment
The borrower fails to make the scheduled payment for their personal loan on the due date.

### Step 2: Reminder Communication
The lender initiates contact with the borrower through various means such as phone calls, emails, or SMS.
They remind the borrower about the missed payment and request immediate payment.

### Step 3: Penalty Charges and Late Fees
If the borrower fails to make the payment within the due date, the lender applies penalty charges and late fees as per the terms and conditions of the loan agreement.
These additional charges are added to the outstanding amount.

### Step 4: Grace Period
Some lenders may provide a grace period during which the borrower can make the overdue payment without being labeled as a defaulter.
The duration of the grace period varies depending on the lender's policies.

### Step 5: Collection Calls and Letters
The lender escalates the collection efforts by making repeated phone calls to the borrower and sending written notices or collection letters.
These communications highlight the consequences of non-payment and urge the borrower to settle the outstanding amount.

### Step 6: Legal Notice
If the borrower still does not respond or make the required payment, the lender may send a legal notice to the borrower.
The legal notice informs the borrower of the intention to take legal action if the debt remains unpaid.

### Step 7: Recovery or Collection Agency
In certain cases, the lender may engage a third-party recovery or collection agency to assist in recovering the outstanding amount.
The agency may contact the borrower through phone calls, letters, or personal visits to enforce payment.

### Step 8: Credit Score Impact
The borrower's non-payment and default are reported to credit bureaus, which negatively impact their credit score.
A lower credit score makes it difficult for the borrower to secure future loans and may also affect their eligibility for other financial services.

### Step 9: Potential legal Proceedings
If all attempts to recover the outstanding amount fail, the lender may initiate legal proceedings against the borrower.

The illustration of a loan default workflow is shown below.

```mermaid
    sequenceDiagram
    Borrower->>Lender: Missed Payment
    Lender->>Borrower: Reminder Communication
    Borrower->>Lender: No Payment Made
    Lender->>Borrower: Grace Period
    Borrower->>Lender: No Payment Made within Grace Period
    Lender->>Lender: Apply Penalty Charges and Late Fees
    Lender->>Borrower: Collection Calls and Letters
    Borrower->>Lender: Non-responsive or No Payment Made
    Lender->>Borrower: Legal Notice
    Borrower->>Lender: No Payment Made after Legal Notice
    Lender->>Lender: Engage Recovery or Collection Agency
    Lender->>Agency: Assign Debt Collection
    Agency->>Borrower: Contact Borrower for Payment
    Borrower->>Agency: Non-responsive or No Payment Made
    Lender->>Lender: Report Default to Credit Bureaus
    Lender->>Lender: Initiate Legal Proceedings
    Lender->>Court: File Case against Borrower
    Court->>Borrower: Serve Court Summons
    Lender->>Lender:Execute Recovery Methods (Wage Garnishment, Asset Seizure, etc.)
```



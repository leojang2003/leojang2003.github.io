---
layout: post
title:  "Interchange Concepts"
date:   2019-10-02 11:45:00 +0800
categories: MasterCard, Interchange
tags: master-card
---

此文章來源為 Interchange Manual 的 Chapter 1

<br />

#### 名詞解釋

interchange rate = 發卡行回佣，國內來說為 1.55%

<br />

#### 關於 Interchange Programs

<br />

> Interchange programs specify the criteria that a transaction must meet to qualify for its associated interchange fee (rate).
Mastercard applies interchange rates as part of the clearing and settlement process. The following characteristics of the transaction determine the interchange rate that is applied to the transaction:

交換程式 ( Interchange programs ) 指出要滿足特定 interchange fee 必須符合的條件，MasterCard 在清算的過程會套用 interchange rate 至各交易。以下屬性決定要套用哪種 interchange rate 至各交易

<br />

> - The card program identifier and business service arrangement 
> - The interchange rate designator (IRD) (PDS 0158 [Business Activity], subfield 4 [Interchange Rate Designator]). 

<br />
<br />

#### 關於 Interchange Rate Designator

<br />

> An IRD represents an interchange program and its associated interchange rate.

**IRD** 代表一個 *interchange program* 及發卡行回佣 

<br />

> Transaction type is criteria that GCMS considers when qualifying transactions for interchange
programs. A transaction type Is identified by DE 3 (Processing Code), subfield 1 (Cardholder Transaction Type)

Transaction type 與 **IRD** 有關，Transaction type 在 DE 3 的 Subfield 1

<br />

> Key criteria in interchange processing are the regions in which transactions are issued and
acquired.

區域與 **IRD** 有關

<br />

#### Card Program ID

<br />

> Card program ID is a criterion that GCMS considers when qualifying transactions for interchange programs.

GCMS 決定交易適用的交換程式 ( Interchange programs ) 會參考 Card Program ID

<br />

> GCMS can recognize cards that have more than one card program identifier association for a single issuing account range. Based on information in the message and parameters maintained at Mastercard, GCMS determines the most appropriate card program identifier to use (in PDS 0158 [Business Activity], subfield 1 [Card Program Identifier]) with the transaction if the transaction is not submitted by the originator of the message.

一個卡號可能有對應多個 Card Program ID，GCMS 會決定最合適的 Card Program ID ( 在 PDS 0158 [Business Activity] 的 subfield 1 [Card Program Identifier] )。下面是 PDS 0158 的範例，這邊 Card Program Identifier 放的是 MCC

<br />

> P0158 S01     MCC  
> P0158 S02  
> P0158 S03  
> P0158 S04     34  

<br />

#### Card Program ID 與 GCMS Product ID

> In addition to the business service and interchange rate designator (IRD), the combination of card program ID and GCMS product ID also may be a factor in qualifying a transaction for a particular interchange program.

除了 business service 與 interchange rate designator (IRD) 之外，card program ID 與 GCMS product ID 的組合也會影響交易所選擇的 interchange program。

<br />

> Each issuer’s account range is assigned one or more card program IDs, each within an associated GCMS product ID. This information is stored at Mastercard and provided to customers in the IPM Mastercard Parameter Extract (MPE) file. GCMS uses this information to help determine whether a transaction qualifies for the submitted IRD.

每個卡號有對應一個或多個 Card Program ID，每個 Card Program ID 對應一個 GCMS product ID。此資訊存在 MasterCard 提供給客戶的 IPM Mastercard Parameter Extract (MPE) 檔案。GCMS 利用此資訊決定交易是否符合送出的 IRD。

<br />

#### Interchange Program Qualifying Criteria

<br />

> Certain transaction criteria determine the interchange program for which a transaction may
qualify. The following criteria qualify transactions for interchange programs.

特定交易準則會決定該交易適用的 interchange program，以下為 interchange program 會參考的準則

<br />
<br />

**Criteria/Description**

_Card Program Identifier_

The set of card program identifiers for which the interchange rate designator (IRD) is valid. Values include, but are not limited to:
- DMC—Debit Mastercard
- MCC—Mastercard Credit
- MSI—Maestro
- PVL—Private label

Card Program Identifiers 包含 DMC,MCC, MSI, PVL, 最常見的就是 MCC

**Clearing Message Element**

PDS 0158 (Business Activity), subfield 1 (Card Program Identifier) 

If not submitted in the message, GCMS derives this value from:
- DE 2 (Primary Account Number [PAN]), or
- DE 93 (Transaction Receiver Institution ID Code) when DE 2 is not present in the message (Fee Collections/1740 [Other] only)

如果 Card Program Identifier 沒帶值，則從 DE 2 或 DE 93 取得

<br />
<br />

**Criteria/Description**

_GCMS Product Identifier_

The set of clearing product identifiers for the IRD.
The set includes all account range products currently being masked under the specific GCMS Product Identifier.

**Clearing Message Element**

- PDS 0002 (GCMS Product Identifier)
- Derived from DE 2 (Primary Account Number[PAN])

<br />
<br />

**Criteria/Description**

_Transaction Type_

The transaction types for which the IRD is valid. GCMS validates the combination of MTI, Function Code, Processing Code, and Reversal Indicator to help ensure the transaction is valid for the IRD.

The DE 3 criteria refers to DE 3, subfield 1(Cardholder Transaction Type). Unless otherwise stated, DE 3 subfields 2 and 3, (Cardholder From Account and Cardholder To Account) must
contain zeros.

Unless otherwise stated it is assumed that both
originals and reversals are permitted.

**Clearing Message Element**

Transaction Function
- Transaction Function (MTI + DE 24 [Function Code])
- DE 3 (Processing Code)
- PDS 0025 (Message Reversal Indicator), subfield 1 (Message Reversal Indicator)

<br />
<br />

**Criteria/Description**

_Card Acceptor Business (CAB) Program_

**Clearing Message Element**

DE 26 (Card Acceptor Business Code [MCC])

<br />
<br />

**Criteria/Description**

_Timeliness_

**Clearing Message Element**

- DE 12 (Date and Time, Local Transaction)
- PDS 0105 (File ID), subfield 2 (File Reference Date)
- PDS 0158 (Business Activity), subfield 4(Interchange Rate Designator)

<br />
<br />

**Criteria/Description**

_Approval Code_

**Clearing Message Element**

DE 38 (Approval Code)

<br />
<br />

**Criteria/Description**

_Trace ID_

**Clearing Message Element**

DE 63 (Transaction Life Cycle ID), subfield 2 (Trace ID) consisting of the Banknet Reference Number(nine positions) followed by the Banknet Date(four positions in the format MMDD)

<br />
<br />

**Criteria/Description**

_Point of Service Data Code_

**Clearing Message Element**

DE 22 (Point of Service Data Code)

<br />
<br />

**Criteria/Description**

_Card Acceptor Business Code (MCC)_

**Clearing Message Element**

DE 26 (Card Acceptor Business Code)

<br />
<br />

**Criteria/Description**

_Amount Tolerance_

**Clearing Message Element**

DE 4 (Amount, Transaction)

<br />
<br />

**Criteria/Description**

_Service Code_

**Clearing Message Element**

DE 40 (Service Code)

<br />
<br />

**Criteria/Description**

_Card Acceptor ID Code_

**Clearing Message Element**

DE 42 (Card Acceptor ID Code)

<br />
<br />

**Criteria/Description**

- _Card Acceptor Name_
- _Card Acceptor Street Address_
- _Card Acceptor City_
- _Card Acceptor Postal (ZIP) Code_
- _Card Acceptor State, Province, or Region Code_
- _Card Acceptor Country Code_

**Clearing Message Element**

DE 43

<br />
<br />

**Criteria/Description**

_Mastercard Assigned ID_

**Clearing Message Element**

PDS 0176 (Mastercard Assigned ID)

<br />
<br />

**Criteria/Description**

_Financial Detail Addendum/1644_

**Clearing Message Element**

Various elements within the Financial Detail Addendum/1644, and, when applicable, the associated First Presentment/1240







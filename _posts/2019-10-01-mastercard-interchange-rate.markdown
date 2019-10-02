---
layout: post
title:  "MasterCard"
date:   2019-10-01 16:46:00 +0800
categories: MasterCard
tags: master-card
---

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

#### **Card Program ID**

<br />

> Card program ID is a criterion that GCMS considers when qualifying transactions for interchange programs.

GCMS 決定交易適用的交換程式 ( Interchange programs ) 會參考 Card Program ID

<br />

> GCMS can recognize cards that have more than one card program identifier association for a single issuing account range. Based on information in the message and parameters maintained at Mastercard, GCMS determines the most appropriate card program identifier to use (in PDS 0158 [Business Activity], subfield 1 [Card Program Identifier]) with the transaction if the transaction is not submitted by the originator of the message.

<br />

一個卡號可能有對應多個 Card Program ID，GCMS 會決定最合適的 Card Program ID ( 在 PDS 0158 [Business Activity] 的 subfield 1 [Card Program Identifier] )。下面是 PDS 0158 的範例，這邊 Card Program Identifier 放的是 MCC

> P0158 S01     MCC  
> P0158 S02  
> P0158 S03  
> P0158 S04     34  

<br />




---
layout: post
title:  "MasterCard IRD"
date:   2019-10-01 16:46:00 +0800
categories: MasterCard
tags: master-card
---

interchange rate = 發卡行回傭，國內來說為 1.55%

#### 關於 Interchange Programs

Interchange programs specify the criteria that a transaction must meet to qualify for its associated interchange fee (rate).
Mastercard applies interchange rates as part of the clearing and settlement process. The following characteristics of the transaction determine the interchange rate that is applied to the transaction:

交換程式 ( Interchange programs ) 指出要滿足特定 interchange fee 必須符合的條件，MasterCard 在清算的過程會套用 interchange rate 至各交易。以下屬性決定要套用哪種 interchange rate 至各交易

- The card program identifier and business service arrangement 
- The interchange rate designator (IRD) (PDS 0158 [Business Activity], subfield 4 [Interchange Rate Designator]). 

<br />
<br />

#### 關於 Interchange Rate Designator

<br />

> An IRD represents an interchange program and its associated interchange rate.

IRD 代表一個 interchange program 及發卡行回傭 

<br />

> Transaction type is criteria that GCMS considers when qualifying transactions for interchange
programs. A transaction type Is identified by DE 3 (Processing Code), subfield 1 (Cardholder Transaction Type)

Transaction type 與 IRD 有關，Transaction type 在 DE 3 的 Subfield 1

<br />

> Key criteria in interchange processing are the regions in which transactions are issued and
acquired.

區域與 IRD 有關

<br />








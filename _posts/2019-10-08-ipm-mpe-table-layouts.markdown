---
layout: post
title:  "Interchange Rates"
date:   2019-10-02 11:45:00 +0800
categories: MasterCard, Interchange
tags: master-card
---

此文章來源為 Interchange Manual 的 Chapter 2

<br />

### 關於 About IPM Member Parameter Extract

<br />

> The IPM Member Parameter Extract (MPE) contains all of the parameters that the Clearing Optimizer uses to edit the member’s clearing files and run the IPM utilities.
If there is no data for a specific table, there will be no table entry. In the event that there are no changes to the IPM MPE for a given day, the file will still be sent to the members
containing a 0000 table as well as the header and trailer.

簡單來說，IPM Member Parameter Extract (MPE) 包含了所有 Clearing Optimizer 編輯清算檔案所需要的所有參數

<br />

#### IPM MPE Definitions

> A MPE table is a unit of the IPM MPE that provides data on a particular set of parameters,which affect the Clearing Optimizer and Clearing System programs. For example, the
Merchant Category Codes (MCC) table provides data related to the Card Acceptor Business Codes (MCCs) that Mastercard supports. The IPM MPE comprises many tables. 
滿足下列 interchange program 的交易適用以下費率

一個 **MPE table**  是 IPM MPE 的一個單位，提供一組特定的參數的資料，用於 Clearing Optimizer 以及 Clearing System programs。舉例來說，Merchant Category Codes (MCC) 資料表提供資料與 MasterCard 所支援的ㄓ的
Card Acceptor Business Codes 有關。The IPM MPE 由許多的資料表所組成。

<br />

> A MPE record is a unit of an IPM MPE table that provides data on a particular parameter. For example, within the Data Element Attributes table, each record provides data on a single data
element.

一筆 **MPE record** 是一個 IPM MPE 資料表的一個單位，提供特定參數的一筆資料。

<br />

> Each MPE record represents a single row of the MPE table in the member’s Parameter Master File.

每筆 **MPE record** 代表會員的參數主檔 (Parameter Master File) 中 MPE 資料表的一筆資料行。

<br />

> The key for each MPE record is a combination of Effective Timestamp, Active-Inactive Code, Copy Member (formerly known as “copybook”), and the fields uniquely identify the record
data. (For example, within the Data Element Attributes Table, the Data Element Number is essential to identify a particular record.)

每筆 MPE record 的 **key** 是由一串東西組成。

**Applying an Inactive IPM MPE Update Record**

> Each IPM MPE table has a unique key that consists of several contiguous fields. The keys for all of the tables are defined in table IP0000T1 Keys, which is the first table in the IPM MPE file.
Customers must ensure that they only apply an “inactive” record if they have an existing record with an identical key starting at position 12. Since positions 1–11 contain the effective
date/time and the active/inactive code, these positions will not match an existing entry and can be disregarded. Following are some examples:

每個 IPM MPE 資料表皆有一個包含數個連續欄位的特殊 key，所有表格的 keys 是定義在資料表 IP0000T1 Keys，是該 IPM MPE 檔案的第一個資料表。

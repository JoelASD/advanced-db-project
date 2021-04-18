# Advanced Databases Project: Online Store

- Joel Aalto, Samson Azizyan, Jaber Askari

The goal is to create the database for the template of the online store,
that can be personalized in the future for various types of products / services.

# Requirements

## Functional Requirements

### The links do not work yet

| Code |                                  Requirement                                   | Priority |
| :--: | :----------------------------------------------------------------------------: | :------: |
| FR01 |         [ Create an account, login, logout ](../liitteet/f1_login.md)          |   High   |
| FR02 |                 [ Delete an account ](../liitteet/f1_login.md)                 |   High   |
| FR03 |             [ Add product to the basket ](../liitteet/f2_tools.md)             |   High   |
| FR04 |                   [ Manage basket ](../liitteet/f2_tools.md)                   |   High   |
| FR05 |              [ Place an order ](../liitteet/f3_delete_account.md)              |   High   |
| FR06 |     [ Add a discount code to the order ](../liitteet/f3_delete_account.md)     |   Low    |
| FR07 | [ Manage an order, depending on the status ](../liitteet/f3_delete_account.md) |   High   |
| FR08 |               [ Return the product ? ](../liitteet/f4_rating.md)               |  Medium  |
| FR09 |               [ Browse the products ](../liitteet/f4_rating.md)                |   High   |
| FR10 |                 [ Write a review ](../liitteet/f5_comment.md)                  |  Medium  |
| FR11 |     [ Comment on the review of the product ](../liitteet/f6_rentatool.md)      |   Low    |
| FR12 |           [ Browse the order history ](../liitteet/f7_returntool.md)           |  Medium  |
| FR13 |             [ Edit account details ](../liitteet/f7_returntool.md)             |   High   |
| FR14 |                [ Pay the order ](../liitteet/f7_returntool.md)                 |   High   |
| FR15 | [ Post or answer a question about the product ](../liitteet/f7_returntool.md)  |   Low    |
| FR16 |          [ Contact customer service ? ](../liitteet/f7_returntool.md)          |  Medium  |
| FR17 |      [ Sign up for the availability alert ](../liitteet/f7_returntool.md)      |   Low    |
| FR18 |         [ Admin panel: manage products ](../liitteet/f7_returntool.md)         |   High   |
| FR19 |          [ Admin panel: manage users ](../liitteet/f7_returntool.md)           |   High   |
| FR20 |      [ Admin panel: manage discount codes ](../liitteet/f7_returntool.md)      |   Low    |
| FR21 |         [ Admin panle: create bundles ](../liitteet/f7_returntool.md)          |  Medium  |
| FR22 |       [ Admin panle: update order status ](../liitteet/f7_returntool.md)       |   High   |
| FR23 |           [ Admin panle: manage faq ](../liitteet/f7_returntool.md)            |  Medium  |

## Non Functional Requirements

| Code  |              Requirement               | Priority |
| :---: | :------------------------------------: | :------: |
| NFR01 |         Personal data logging          |  Medium  |
| NFR02 |         Product browse history         |  Medium  |
| NFR03 |       Database management roles        |   High   |
| NFR04 |            Database backups            |   High   |
| NFR05 | Prepared statements in the server side |   High   |
| NFR06 |           Generating reports           |   Low    |
| NFR07 |    Responsive database (indexing ?)    |   High   |

# Conceptual Data Model

## Simple Version

![Simple Conceptual Data Model](./images/simple_cdm.JPG)

- Customer can have multiple orders over time
- In single order there must be at least one product
- The QA concept has a many to many relationship to itself, because comments can have comments
- The QA also connects to the customer and the product tables, because the comment can be about product

## Full Version

### Version 1

![Conceptual Data Model](./images/cdm.JPG)

### Version 2

![Conceptual Data Model](./images/cdm_v2.JPG)

# Physical Design Model

## Version 1

![Physical Data Model](./images/pdm.JPG)

## Version 2

[Creation Query](./pages/creation_query.md)

![Physical Data Model](./images/pdm_v2.JPG)

## Metadata

|                        Table                         |
| :--------------------------------------------------: |
|       [ UserAccount ](./pages/UserAccount.md)        |
|          [ Contract ](./pages/Contract.md)           |
|       [ PhoneNumber ](./pages/PhoneNumber.md)        |
| [ Address, ZipCode, City, Country ](./pages/AZCC.md) |
|           [ Session ](./pages/Session.md)            |
|        [ Timestamps ](./pages/timestamps.md)         |
|             [ Order ](./pages/Order.md)              |
|            [ Basket ](./pages/basket.md)             |
|        [ PaymentMethod ](./pages/payment.md)         |
|          [ Shipment ](./pages/shipment.md)           |
|          [ GiftCard ](./pages/giftcard.md)           |
|          [ Discount ](./pages/discount.md)           |

## Database Management Plan

For the purpose of this project we used the MySQL Workbench as our database administration tool and PHPMyAdmin for testing and as an CRUD prototype.

## indexing

To have a more efficient database we decided to create indexes for Product, Order and UserAccount tables. Indexes will help us to retrieve data from
database quickly. One important thing in indexing is that all the foreign keys should be indexed in order to improve performance.

**Index for Product:**

```sql
SELECT ProductTitle, Price, ProductCategoryID FROM Product Where ProductTitle="camera" ORDER BY Price;
CREATE INDEX index_product ON Product (ProductTitle, Price, ProductCategoryID);
```

Usage

- This index will be used in [functional requirement FR09](./pages/req/FR09.md) to help users to browse the products with out long delay.
- It will be used in [functional requirement FR18](./pages/req/FR18.md) to help the admin user to manage the products.
- Sorting the products based on price an name will be faster.

**Index for Order:**

```sql
SELECT OrderStatus, PaymentMethodID, BasketID FROM Order Where OrderStatus="completed";
CREATE INDEX index_Order ON Order (OrderStatus, PaymentMethodID, BasketID);
```

Usage

- the index will be used in [functional requirement FR07](./pages/req/FR07.md) when an order needs to managed, depending on its status.

**Index for UserAccount:**

```sql
SELECT FirstName, LastName, Email FROM UserAccount Where Email="email@gmail.com" AND LastName="admin" AND FirstName="admin" ORDER BY FirstName;
CREATE INDEX index_UserAccount ON UserAccount (Email, FirstName, LastName);
```

Usage:

- The index will be used in [functional requirement FR01](./pages/req/FR01.md) when a user wants to login/logout.
- The index will be used in [functional requirement FR02](./pages/req/FR02.md) when an account needs to be deleted.
- The index will be used in [functional requirement FR13](./pages/req/FR13.md) when a user needs to edit account details.
- The index will be used in [functional requirement FR19](./pages/req/FR19.md) when a Admin user needs to manage users.

### Creating backups (how, when, where, by whom)

Daily backups at 00:00 to the separate virtual instance through ssh.

### Partitioning, disk usage \*

Since the Order table is going to increase in size rapidly through time, it would good practice to partition the Order table and other tables which are related to it by year (horizontal partition). It means, that for each year we create a new partition for Order, Basket, PaymentMethod, Shipment tables.

### DB optimization (creating indexes etc.)

- Create indexes for the most frequently used tables
- Use transactions for the payment process and adding products to the basket
- Create some triggers (review stars amount)

### Maintenance of database

**Index defragmentation:**
In order to fix fragmentation, our maintenance plan would evaluate each index for its size, use, and fragmentation level and will either perform a INDEX_REORGANIZE, an INDEX_REBUILD_ONLINE, or even an INDEX_REBUILD_OFFLINE depending how bad the index is.

**Log file maintenance:**
MariaDB has the option to log everything on the server by using the general log file.
At the end of [the database creation query](pages/creation_query.md) we added two commands:

```sql
SET GLOBAL general_log=1; --Enabling logging
SET GLOBAL general_log_file='online-store.log'; --Renaming the log file
```

**Data compaction:**
Data compaction reorganizes the data the way that the data from the same table would be located closer together.

**Integrity check:**
Over time the database will go through a lot of changes, every change can corrupt the database. The integrity check examines and analyses the whole database and can repair all the corruptions.

### Users, user roles and their rights

- Database Administrator
- Backend Developer

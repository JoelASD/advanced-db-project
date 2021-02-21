# Advanced Databases Project: Online Store

* Joel Aalto, Samson Azizyan, Jaber Askari

The goal is to create the database for the tempalate of the online store,
that can be personalized in the future for various types of products / services.

# Requirements

## Functional Requirements

### The links dont work yet

| Code | Requirement | Priority |
| :-: | :-: | :-: |
| FR01 | [ Create an account, login, logout ](../liitteet/f1_login.md) | High |
| FR02 | [ Delete an account ](../liitteet/f1_login.md) | High |
| FR03 | [ Add product to the basket ](../liitteet/f2_tools.md) | High |
| FR04 | [ Manage basket ](../liitteet/f2_tools.md) | High |
| FR05 | [ Place an order ](../liitteet/f3_delete_account.md) | High |
| FR06 | [ Add a discount code to the order ](../liitteet/f3_delete_account.md) | Low |
| FR07 | [ Manage an order, depending on the status ](../liitteet/f3_delete_account.md) | High |
| FR08 | [ Return the product ? ](../liitteet/f4_rating.md) | Medium |
| FR09 | [ Browse the products ](../liitteet/f4_rating.md) | High |
| FR10 | [ Write a review ](../liitteet/f5_comment.md) | Medium |
| FR11 | [ Comment on the review of the product ](../liitteet/f6_rentatool.md) | Low |
| FR12 | [ Browse the order history ](../liitteet/f7_returntool.md) | Medium |
| FR13 | [ Edit account details ](../liitteet/f7_returntool.md) | High |
| FR14 | [ Pay the order ](../liitteet/f7_returntool.md) | High |
| FR15 | [ Post or answer a question about the product ](../liitteet/f7_returntool.md) | Low |
| FR16 | [ Contact customer service ? ](../liitteet/f7_returntool.md) | Medium |
| FR17 | [ Sign up for the availability alert ](../liitteet/f7_returntool.md) | Low |
| FR18 | [ Admin panel: manage products ](../liitteet/f7_returntool.md) | High |
| FR19 | [ Admin panel: manage users ](../liitteet/f7_returntool.md) | High |
| FR20 | [ Admin panel: manage discount codes ](../liitteet/f7_returntool.md) | Low |
| FR21 | [ Admin panle: create bundles ](../liitteet/f7_returntool.md) | Medium |
| FR22 | [ Admin panle: update order status ](../liitteet/f7_returntool.md) | High |
| FR23 | [ Admin panle: manage faq ](../liitteet/f7_returntool.md) | Medium |

## Non Functional Requirements

| Code | Requirement | Priority |								
|:-:|:-:|:-:|
| NFR01 | Personal data logging | Medium |
| NFR02 | Product browse history | Medium |
| NFR03 | Database management roles | High |
| NFR04 | Database backups | High |
| NFR05 | Prepared statements in the server side | High |
| NFR06 | Generating reports | Low |
| NFR07 | Responsive database (indexing ?) | High |

# Conceptual Data Model

## Simple Version

![Simple Conceptual Data Model](./images/simple_cdm.JPG)

* Customer can have multiple orders over time
* In single order there must be at least one product
* The QA concept has a many to many relationship to itself, because comments can have comments
* The QA also connects to the customer and the product tables, because the comment can be about product

## Full Version

### Version 1

![Conceptual Data Model](./images/cdm.JPG)

### Version 2

![Conceptual Data Model](./images/cdm_v2.JPG)

# Physical Design Model

![Physical Data Model](./images/pdm.JPG)

## Database Management Plan

### Creating backups (how, when, where, by whom)
Daily backups at 00:00  to the separate virtual instance through ssh.

### Partitioning, disk usage *
Partition the order table by years ?

### DB optimization (creating indexes etc.)
* Create indexes for the most frequently used tables
* Use transactions for the payment process and adding products to the basket
* Create some triggers (review stars amount)

### Maintenance of database

**Index defragmentation:**
In order to fix fragmentation, our maintenance plan would evaluate each index for its size, use, and fragmentation level and will either perform a INDEX_REORGANIZE, an INDEX_REBUILD_ONLINE, or even an INDEX_REBUILD_OFFLINE depending how bad the index is.

**Log file maintenance:**
SQL databases contain a log file, that stores all the transactions made in the database. With this log file you can restore the database to its previous state.

**Data compaction:**
Data compaction reorganizes the data the way that the data from the same table would be located closer together.

**Integrity check:**
Over time the database will go through a lot of changes, every change can corrupt the database. The integrity check examines and analyses the whole database and can repair all the corruptions.


### Users, user roles and their rights
* Database Administrator
* Backend Developer

# Advanced Databases Project: Online Store

# Requirements

## Functional Requirements

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

## Ei-toiminnallisia suorituskykyvaatimuksia

| VaatimusID | Kuvaus | Ominaisuus johon vaikuttaa |								
|:-:|:-:|:-:|
| SYSTEM-REQ-0001 | Palvelun pitää pystyä käyttämään käyttäjän sijaintia |FT02, FT04|
| SYSTEM-REQ-0002 | Palvelun pitää pystyä käyttämään päätelaitteen kameraa |FT04|
| USAGE-REQ-0001 | Kirjatumispalvelulla on oltava itsenäinen kolmannen osapuolen toimittama varmistusjärjestelmä | [FT01](liitteet/f1_login.md) |
| USAGE-REQ-0002 | Palvelun pitää olla tarjolla eri alustoilla ||
| SEC-REQ-0001 | Kirjatumispalvelulla on oltava google authenticator | [FT01](liitteet/f1_login.md) |
| SEC-REQ-0002 | Tietokannan tulee olemaan suojattuna suoajausalgoritmilla ||

# Conceptual Data Model

# Logical Data Model
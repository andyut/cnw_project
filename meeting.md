# Pembelian Plastik

## Skema 1
### Masuk dalam stock SAP
```mermaid
graph TD
A[ PLASTIK IN] --> B(PO / AP INVOICE)
B --> C[[STOCK CARD]]

D(PLASTIK OUT) --> E(GOOD ISSUE)
E -->C


```


## Skema 2
###  Biaya di SAP, stock card diluar SAP
```mermaid
graph TD
A[ PLASTIK IN] --> B(CNW EXPENSE)
B --> C[[SAP JURNAL ENTRY BIAYA]]

B -..-> C1[["CNW INVENTORY CONSUME (QTY) "]]
E[PLASTIK OUT] --> E1("CNW STOCK CONSUME (QTY)")
E1 --> C1

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ5NjcwNDM1NiwtMTcwNTY4MDQ5NCwtNz
MyMTc2ODI2LDEyNTMxOTE0MDJdfQ==
-->
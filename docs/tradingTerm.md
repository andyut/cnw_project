## Trading Term monitoring SAP

> Modul Trading Term SAP



**Keterangan**




### Alur Kerja


#### Sales

1. Sales Input Trading term sesuai dengan kontrak customer 

> **TRDM -> List Trading Term**


![Vendor Numbering](img/tradingterm_tree.png)

> **Klik Create**

![Vendor Numbering](img/tradingterm_form.png)

#### Accounting
- Business Partner
- group Barang
- Sub Group Barang
- Expense Item

```mermaid

erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE-ITEM : contains
    CUSTOMER }|..|{ DELIVERY-ADDRESS : uses
```

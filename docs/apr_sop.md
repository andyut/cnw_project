## Trading Term monitoring SAP

> Modul Trading Term SAP



---

### Modul

 
#### Trading Term
#### Setup Master 
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

## Pembelian Aktiva SAP 

**Keterangan :**

Digunakan untuk pembelian ATK, ASET, Mesin dll

    __Semua pembayaran untuk pembelian Asset harus menyertakan PO SAP__

### Konfigurasi 

**[SAP B1]**

* Buat Group Non Vendor Dengan kode VO

![Vendor Numbering](img/vendor_numbering.png)



* Buat COA baru hutang non barang dagang

![Hutang Activa](img/coa_hutangactiva.png)



* Buat kelompok Item Baru dengan ketentuan 
    * Checklist inventory dimatikan
    * Group Accounting lari ke biaya

![Item Activa](img/item_activa.png)
 



### Prosedur

1. Bagian Procurement /purchasing membuat form _purchase request_
2. Kemudian diserahkan ke bagian _Accounting_ untuk dicek apakah aktiva atau biaya
3. Kemudian bagian _Accounting_ membuat **Fixed Asset Master Data** di SAP sesuai dengan Kriteria Asset yang ditentukan

```mermaid

graph TD

B(Procurement/Purchasing) --"Form Request" --> A{Asset?}
A --Y--> C(FixAsset Master data) 
C ==Asset_Code==> B

```
4. kemudian **Kode Master Data** tersebut diberikan ke bagian Procurement untuk dibuatkan PO
5. Bagian Procurement / Purchasing membuat Purchase Order sesuai kode yang dibuat oleh bag _Accounting_
6. Purchase Order tersebut diserahkan ke bagian Accounting untuk diproses pembayaran dan pengakuan assetnya


```mermaid

graph TD

B(Procurement/Purchasing) --"Purchase Order" --> A(Accounting)
A --Payment--> C(Pembayaran / jurnal Manual)
A --Pengakuan Asset--> D(AP Invoice)

```
    Jurnal yang terbentuk :
        [1] UM Aktiva 
                Hutang Aktiva
                
        [2] Asset 
                UM Aktiva

    Pembayaran dibayar dengan jurnal Manual :
        [3] Titipan 
                Bank

7. Setelah barang lengkap, bagian accounting membuat **AP Invoice** copy from **PO** Asset tersebut


            
```mermaid

graph TD

B(AP Invoice) --> A(Outgoing Payment)
```

    Pembayaran dibayar dengan Outgoing Payment :
        [4] Hutang Aktiva
                Titipan

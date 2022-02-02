## Pembelian Aktiva SAP 

**Keterangan :**

Digunakan untuk pembelian ATK, ASET, Mesin dll



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

A. Bagian Procurement /purchasing membuat form _purchase request_
B. Kemudian diserahkan ke bagian _Accounting_ untuk dicek apakah aktiva atau biaya
C. Kemudian bagian _Accounting_ membuat **Fixed Asset Master Data** di SAP, kemudian **Kode Master Data** tersebut diberikan ke bagian Procurement untuk dibuatkan PO



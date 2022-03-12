# CNW JE ( Jurnal Entry SAP)
## Petunjuk Penggunaan


###### Generated Using : *Markdown Mermaid*
###### Version : *1.0*
  
---

---
## Keterangan  

CNW JE modul yang digunakan untuk memasukan **Jurnal Entry SAP** menggunakan ***web Services Intidata***.  Mencakup Jurnal entry, cost center, control account, cetakan vocher.

### Menu 
```mermaid
graph TD 

A(CNW JE) --> D(SAP Jurnal Entry)
A(CNW JE) --> E(Account Setting)
```

### Sub Menu

```mermaid
graph LR 
C(SAP Jurnal Entry) --> C1(Journal Entry)
D(Account Setting) --> D1(Account)
D(Account Setting) --> D2(Department)
D(Account Setting) --> D3(Divisi)
D(Account Setting) --> D4(Expense Type)
D(Account Setting) --> D5(SAP Business Partner)


```


## SAP Jurnal Entry -> Jurnal Entry

### Form View
![Odoo](img/je_form_view.jpeg)

Mapping dengan SAP Business One

![SAP B1](img/je_sap_view.jpeg)



### Field 
**(26) Name** : Nomor Internal Jurnal Entry CNW
**Company** : Company yang aktif
**Doc Date** : Tanggal dokumen
**(2) Type** : Jenis Jurnal Entry


Daftar Jenis Tipe Jurnal Entry :
* JL : Jurnal Lain
* JV : Jurnal Penyesuaian
* BK : Bank Kredit
* BD : Bank Debet
* KK : Kas Kredit
* KD : Kas Debet


	Jika pilihan BK, BD, KD, KK maka cetakannya akan muncul jenis cetakan *Voucher pengeluaran/penerimaan kas/bank*. Jika pilihannya JV,JL maka cetakannya akan muncul cetakan *Jurnal Penyesuaian / Pemindahan*


```mermaid
graph TD
A[JE] --> B{Type?}
B -->|BK, BD,KK,KD| C[Kasbon Penerimaan / Pengeluaran]
B -->|JV,JL| D[Jurnal Penyesuaian / Pemindahan]

```

Penomoran SAP berdasarkan pilihan Type ini 

```python
[bk] = [BK]{year}{month}[999999]
[bd] = [BD]{year}{month}[999999]
[kk] = [KK]{year}{month}[999999]
[kd] = [KD]{year}{month}[999999]
[jv] = [JV]{year}{month}[999999]
[jl] = [JL]{year}{month}[999999]
```

**(3) Is Remark Printed** : Remark di line di print pada voucher di Jurnal Penyesuaian, jika tidak di ==Thick==  maka remark nya dicetak  

**(4) Is SAP Partner** : Berhubungan dengan SAP business Partner atau tidak


```mermaid
graph TD
A --> B{Is SAP Partner?}

B -->|No| C(Input Manual Partner atau dikosongkan ==Optional== )
B -->|Yes| D(Pilih list Partner dari drownDown menu)
D -->E{Tidak ada Di List}
E -->|Yes| F(Load / Refresh BP di menu Account-Setting)
```

Catatan : Untuk COA Tipe **Control Account** ,  Check List ini harus posisi **Yes**, karena kode BP tersebut akan masuk ke dalam ***CardCode*** Didalam Table Jurnal entry (  *JDT1* )

**(5) Partner SAP** : Dropdown untuk memilih SAP Partner, jika tipe Account menggunakan **Control Account**  maka akan masuk ke dalam CardCode di JDT1 , dan Business Card ID di UDF JDT1.  Jika Tidak menggunakan **Control Account** maka akan ditempel nama customer di field remarks

**(5) Other Partner** : Partner diluar SAP business One Partner.  
**(6) Tunai/Cheque No** : Kolum untuk memasukan Nomor Cheque .  
**(7) Invoice Ref / Notes** : Nomor Invoice Referensi
**(1) Remark 1** : Kolom keterangan ==(Wajib Diisi)==.  
**(1) Remark 2** : Kolom keterangan tambahan (baris 2).  
**(1) Remark 3** : Kolom keterangan tambahan (baris 3) .  
**(8) Remark 4** : Kolom keterangan Dalam bentuk HTML .  
**(10) Amount** :  Total nilai dari baris yang di *thick* "Printed as Total in Kasbon"
**(11) transID** :  Nomor ID JE SAP Business One  
**(12) Number in SAP** : Kode ==Number==  JE Header SAP Business One  
**(13) U_Trans_No** : Nomor Transaksi Akuntansi SAP Business One  
**(14) Debit, Credit, Balance** : Nilai dibaris Jurnal Entry, Balance **harus = 0**  
**(15) Account** :Account dari SAP Business One    
**(16) Remark** : Keterangan baris   
**(17) Printed as Total In Kasbon** : Jika checklist, maka akan dihitung nilai dari *absolut (Debet - Credit)* , dan terbilangnya. nilai ini yang akan muncul di cetakan kasbon ( khusus untuk BK , BD, KK , KD)    
**(18) Printed in Detail** :  Baris ini dicetak saat print perincian detail jurnal Entry
**(19) Debit** : Nilai debet   
**(20) Credit** : Nilai credit   
**(21) Departmen, (22) Divisi , (23) Expense Type** : Departmen, Divisi dan Expense Type dari SAP   

```mermaid
graph TD
A[Department] --> B{Tidak ada}
B -->|Y| C(Reload dari SAP)

D[Divisi] --> B1{Tidak ada}
B1 -->|Y| C(Reload dari SAP Business One)

E[Expense Type] --> B2{Tidak ada}
B2 -->|Y| C2(Reload dari SAP WEB)
```
**(24) Status** : Status dokumen JE ( Draft, Approved, Posted to SAP)

```mermaid
stateDiagram-v2
[*] --> Draft
Draft --> Approved
Approved --> Posting_To_SAP
Posting_To_SAP --> [*]
Posting_To_SAP --> Delete_from_SAP 
Posting_To_SAP --> Edit_from_SAP 
Edit_from_SAP --> Approved
Delete_from_SAP --> Approved
```

**(25) Tombol Action** : Tombol Action  


<!--stackedit_data:
eyJoaXN0b3J5IjpbMzM4NTAyMTQ3LC0xNTk1MjIzNTA3LC01MD
MzNzE5NjcsLTczNzMxNjM2MywxNjU0NzExODEsLTEwMzk5MDkz
MzIsMTgxMDAwNzg0MywyMDY3MTI5Mzk2LDE1MDE1ODE5NDMsMT
QxMDQ4NTA3MywyMTE0NTAzNTEwLC0xNTQ5MzkxMjIxLC0xNzgz
Mzg2Mjc1LC0yMDQ4MjMzMTQxLC0xNDgyODUwMDA4LC02ODgyNT
EyMTIsLTg5NTY0NzI3MiwxMTAwODM4NzY2LC0xMTYwMDcxNDU5
LDE4NTgwNDMxOF19
-->
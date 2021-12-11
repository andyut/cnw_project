# SAP Pembelian  
## Checklist dan troubleshoot


## Gambaran Umum

Di SAP Business One Pembelian terdiri dari 8 komponen

|SAP Transtype Code | Name |Table Name |
| ------ | ------| ------|
| 22 | Purchase Order|OPOR|
| 20 | Good Receipt PO|OPDN|
| 18 | AP Invoice|OPCH|
| 19 | AP Credit Memo|ORPC|
| 21 | Good Return|ORPD|
| 30 | Jurnal Entry|JDT1|
| 69 | Landed Cost|IPF|

Semua komponen diatas bermuara di 2 table utama SAP, yaitu OINM ( Inventory Audit Report) , dan JDT1 ( Jurnal Entry)

```mermaid
graph BT
A[OPDN] ---->|createdby| x[OINM]
B[OPCH] ---->|createdby| x[OINM]
C[IPF] ---->|createdby| x[OINM]
D[ORPC] ---->|createdby| x[OINM]
E[ORPD] ---->|createdby| x[OINM]

A[OPDN] -..->|transid| y[JDT1]
B[OPCH] -..->|transid| y[JDT1]
C[IPF] -..->|transid| y[JDT1]
D[ORPC] -..->|transid| y[JDT1]
E[ORPD] -..->|transid| y[JDT1]

```

## Laporan Pembelian 

Dapat Dicek dari 4 bagian modul
* **1. Purchase Analysis SAP**
* **2. Inventory Audit Report Query**
* **3. General Ledger**
* **4. Laporan HPP Global**


**1. Purchase Analysis SAP**

Menu --> Purchase Analysis Item
![Purchase Analysis](https://www.dropbox.com/s/tznp53s7e1n1p5i/PURCHASE%20DATA%20ITEM.png?dl=1)

Menu --> Purchase Analysis  

![Purchase Analysis](https://www.dropbox.com/s/hpcspadwpnxcavv/PURCHASE%20DATA.png?dl=1)


Catatan :
Laporan *Purchase Analysis* SAP Business total tidak termasuk **Freight**, dan **Landed Cost**  

Laporan *Purchase Analysis* dan *Purchase Analysis Item* bisa berbeda karena beberapa hal
* *Adanya AP Services*
* *Adanya AP Credit Memo Services*

Laporan Pembelian SAP berdasarkan AP invoice Dan good Receipt PO. Jika terjadi perbedaan antara GR dan AP , maka akan timbul **jurnal HPP** di AP invoice

```mermaid
graph TD
A(Good Receipt PO) --> A1(AP Invoice)
A --> A2(Landed Cost) 
A2 -->A3{Stock Ada?}
A3 -->|Y| A4(Bebankan Ke Stock)
A3 -->|N| A5(Bebankan Ke HPP Price Difference)
A3 -->|Kurang| A6(Bebankan Sebagian Ke HPP Price Difference dan stock)
 
A1 --> B{Over missing Qty ?}
B -->|Y| C(Rubah nilai sesuai invoice Supplier)
C --> D{Stock Habis}
D-->|Y| E([selisih dibuang ke HPP Price Difference])
D-->|N| F([dibebankan ke barang yang ada])
F--> H[[ Update ke JDT1 ]]
E -->H[[ Update ke JDT1 ]] 
F--> G[[ Update ke OINM ]]
F--> I[[ Update ke OITW - Item Cost ]]


```


**2. Inventory Audit Report Query**

```sql
declare         
			    @v_Pembelian     float	, 
				@v_landed        float	,
				@v_TotalHPP		 float	,
                @v_landedHPP     float
/*
Pembelian dari Inventory Audit Report
*/                
select @v_Pembelian = sum( a.transvalue) 
from    OINM A WHERE   CONVERT( VARCHAR, A.DOCDATE ,112) >= (@datefrom)  and   CONVERT( VARCHAR, A.DOCDATE ,112) <= (@dateto)   AND transtype in (20,19,21,18)

/*
Landed Cost dari Inventory Audit Report
*/                
select @v_landed= isnull(sum( a.transvalue) ,0)
from    OINM A WHERE   CONVERT( VARCHAR, A.DOCDATE ,112) >= (@datefrom)  and   CONVERT( VARCHAR, A.DOCDATE ,112) <= (@dateto)   AND transtype in (69)

/*
Landed Cost dari Jurnal , yang khusus account 5 ( HPP )
*/                
 
select @v_landedHPP  =isnull(sum (debit-credit),0) 
 From jdt1 where left(account,1)='5' 
and  CONVERT( VARCHAR, refdate ,112) >= (@datefrom)  and   CONVERT( VARCHAR,refdate ,112) <= (@dateto) 
and transtype=69

select          'Pembelian',isnull(@v_Pembelian,0) Pembelian 
union all
select          'Landed Cost',isnull(@v_landed,0) Landed  
union all
select          'Landed Cost (HPP)',isnull(@v_landedHPP,0) Landedhpp  
```

**Cek Nilai Landed Cost**

Total Landed A  = Total Landed di Inventory + Total Landed Di GL komponen HPP ( transtype=Landedcost)  
Total Landed B = GL komponen 2140 yang dari jurnal Entry (30)

*Total Landed A = Total Landed B* = *Total Landed Cost*




**3. General Ledger**

Total Pembelian = GL (1190) Komponen (20,19,21,18,69) + GL (5) Komponen  (20,19,21,18,69) 


**4. HPP Global**

Landed Cost HPP = Landed Cost dari GL (5)  yang tidak bisa dibebankan ke item barang  
Landed Cost Item= Landed Cost dari GL (5)  yang  dibebankan ke item barang

Total Pembelian = Total Pembelian + Landed Cost Item + Landed Cost HPP


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU2Mjk1Mjg0Niw4MjIyMDY4NDQsLTIxND
A4MDUyNDgsOTM5ODAxMTQsLTMxODczMDM0LDE5NzEwNjIyMjgs
LTMxODczMDM0LDE3NzQxMDg3OTcsLTEwMjkxODkyOTksLTU5MT
U3OTI4MywtMTU4NTY0MjQ0NSw0NzQ5OTQ2NjQsLTI1Nzc3MDk0
OCwxMjE3ODkxMjMsMjA5MzY2OTgxOSwtNzIxMjE1NjExLC00MD
U5NDA3ODgsLTEwODUxNTE2MzEsLTE2OTIwODU1MzNdfQ==
-->
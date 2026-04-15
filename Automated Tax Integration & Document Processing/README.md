# Project: Automated Tax Integration [Zoho Books to Coretax (Indonesian Government Tax Portal)]

## **Problem/Challenge**
Seorang *Tax Accountant* sebelumnya harus melakukan input data secara manual dari sistem akuntansi ke situs web pemerintah untuk pelaporan pajak. Proses ini tidak hanya memakan waktu yang lama tetapi juga sangat berisiko terhadap kesalahan input data (*human error*). Tantangan utamanya adalah mengekstraksi data transaksi yang relevan secara otomatis dan mentransformasikannya ke dalam format Excel yang kompatibel untuk diekspor menjadi file `.xml` (e-Faktur/e-Bupot) agar siap diunggah ke portal pajak.

## **Solution**
Saya merancang dan membangun alur kerja otomatisasi menggunakan **Power Automate** yang menghubungkan **Zoho Books** dengan ekosistem Microsoft 365. Solusi ini secara otomatis mengambil data organisasi, daftar akun (*Chart of Accounts*), dan detail transaksi, kemudian mengolahnya menggunakan logika kondisi untuk mengisi template laporan pajak secara akurat.

## **Technology**
* **ERP System:** Zoho Books (API Integration).
* **Triggers:** `When an item is created or modified` di SharePoint (sebagai pengontrol eksekusi).
* **Logic & Control:** * **Web Service (REST API):** Menggunakan aksi HTTP untuk *Refreshing Access Tokens* Zoho demi keamanan koneksi.
    * **Switch Case:** Memisahkan logika pemrosesan data berdasarkan tipe transaksi Zoho (Bills, Vendor Payments, Journal).
    * **Variables:** Pengelolaan ID Akun, ID Organisasi, dan penamaan file dinamis.
* **Integrations:** Microsoft SharePoint, Excel Online (Business), dan Power Automate.

## **Workflow Overview**
1.  **Authentication:** Flow dimulai dengan melakukan *refresh access token* Zoho Books untuk memastikan sesi API tetap aktif dan aman.

    ![alt text](1.png)

2.  **Organization Sync:** Menarik data profil organisasi dan *Chart of Accounts* dari Zoho untuk memastikan pemetaan akun pajak sudah benar.

3.  **Dynamic File Handling:** Sistem mengecek apakah file laporan untuk periode tersebut sudah ada di SharePoint. Jika belum, flow akan membuat file baru berdasarkan template master.

    ![alt text](2.png)

4.  **Transaction Sorting (Switch Case):**
    * **Case Bills:** Mengambil data tagihan masuk dari Zoho Books.
    * **Case Vendor Payments:** Mengambil data pembayaran ke vendor.
    * **Case Journal:** Menarik data penyesuaian jurnal umum.

5.  **Data Consolidation:** Setiap baris transaksi dimasukkan ke dalam tabel Excel menggunakan aksi *Add a row into a table*, yang sudah diformat agar siap dikonversi menjadi `.xml`.

    ![alt text](3.png)


## **Impact**
* **Time Efficiency:** Memangkas waktu persiapan data pajak dari yang sebelumnya berjam-jam menjadi hitungan menit.
* **Data Integrity:** Memastikan data yang ada di pelaporan pajak 100% sinkron dengan data yang tercatat di Zoho Books.
* **Standardization:** Menciptakan proses pelaporan yang terstruktur dan dapat diulang (*repeatable*) setiap bulannya tanpa bergantung pada input manual.

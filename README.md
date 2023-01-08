# SIG_Teori_2101092021_Intermediate_Gis_Operations
Intermediate GIS operations
1. Performing Table Joins (QGIS3)
- Temukan tl_2019_06_tract.zipfile di Peramban QGIS dan kembangkan. Pilih tl_2019_06_tract.shpfile dan seret ke kanvas.
- Dialog Select Transformation akan meminta untuk mengkonversi dari EPSG:4269 ke EPSG:4326 . Dialog ini menyajikan beberapa transformasi untuk mengkonversi antara koordinat antara proyeksi tersebut. Tinggalkan pilihan ke pilihan default dan klik OK .
- Anda akan melihat layer tl_2019_06_tractdimuat di panel Layers . Lapisan ini berisi batas-batas bidang sensus di California. Klik kanan pada tl_2019_06_tractlayer dan pilih Open Attribute Table .
- Periksa atribut layer. Untuk menggabungkan tabel dengan lapisan ini, kita memerlukan atribut unik dan umum dari setiap fitur. Dalam hal ini, ada 8057 catatan traktat individu dengan GEOIDbidang tersebut. Kolom ini dapat menautkan lapisan ini dengan lapisan atau tabel lain yang berisi ID yang sama.
- Untuk memuat data tabular, klik Open Data Source Manager .
- Dalam dialog Pengelola Sumber Data , pilih Teks yang Dibatasi . Kemudian di sebelah kanan, klik di ...sebelah Nama file dan ramban ke folder yang tidak di-zip dengan CSV populasi California.
- Sekarang di bawah Sample Data , kita dapat memeriksa data bahkan sebelum memuatnya sebagai layer. Representasi menunjukkan bahwa tabel data berisi 2 baris header.
- Untuk menghilangkan baris header tambahan, di bawah Opsi Rekam dan Bidang atur Jumlah baris header yang akan dibuang ke 1. Sekarang tabel akan berisi tajuk kolom yang tepat. Karena layer ini hanya berisi data tabular, pilih di bawah Geometry Definition . Klik Add untuk menambahkannya sebagai layer dan kemudian klik Close untuk menutup kotak dialog ini.No geometry (attribute only table)
- CSV sekarang akan diimpor sebagai tabel ke QGIS dan muncul seperti ACST5Y2019.S0101pada panel Lapisan . Sekarang klik kanan pada layer dan pilih Open Attribute Table .
- Kolom IDtersebut berisi id unik untuk setiap record, yang dapat digunakan untuk menggabungkan tabel ini dengan tl_2019_06_tractlayer. Jika Anda membandingkan nilai IDdengan GEOIDkolom dari tl_2019_06_tract. Anda akan melihat bahwa itu diawali dengan 1400000US . Untuk menggabungkan kedua tabel ini dengan sukses, nilainya harus sama persis. Mari kita hapus awalan ini dan tambahkan kolom baru dengan 11 karakter terakhir yang berisi nilai yang sama persis.
- Untuk membuat kolom baru dengan 11 digit terakhir, buka Processing Toolbox dengan masuk ke Processing ‣ Toolbox , dan cari dan temukan tabel Vector ‣ Algoritma kalkulator lapangan.
- Dalam dialog Kalkulator bidangACST5Y2019.S0101 , pilih sebagai Input layer , masukkan Nama bidanggeoid , dan pilih Jenis Bidang Hasil . Sekarang cari dalam ekspresi. Kita dapat menggunakan fungsi ini untuk mengekstrak bagian yang diperlukan dari bidang id.stringsubstr
- Masukkan ekspresi di bawah ini. Kami menggunakan fungsi substr dan mengekstrak nilai dari posisi -11 (nilai negatif dihitung dari akhir). Hasil akhir dapat dilihat di bagian Pratinjau . Klik Jalankan .
- Sekarang layer baru Calculatedakan dimuat di kanvas, mari kita periksa tabel atribut. Kolom baru geoiddengan nilai yang dapat dicocokkan dengan risalah pencacahan akan hadir.
- Untuk membuat gabungan tabel, buka Processing Toolbox dengan masuk ke Processing ‣ Toolbox , dan cari dan temukan atribut Vector general ‣ Join dengan algoritma nilai bidang.
- Dalam dialog Gabungkan atribut menurut nilai bidangtl_2019_06_tract , pilih sebagai lapisan Input dan GEOIDsebagai bidang Tabel . Pilih Calculatedsebagai Input layer 2 dan geoidsebagai Table field 2 . Di bawah bidang Layer2 untuk menyalin , klik pada ....
- Periksa , dan . Klik Oke .Geographic Area NameEstimate!!Total!!Total populationgeoid
- Centang catatan Buang yang tidak dapat digabungkan . Ini akan menghilangkan catatan tambahan apa pun di tabel populasi. Klik tombol … di bawah layer gabungan untuk memilih lokasi file keluaran dan pilih .Save to File...
- Beri nama geopackage keluaran sebagai california_total_population.gpkg. Klik Jalankan .
- Setelah pemrosesan selesai, verifikasi bahwa algoritme berhasil jika semua fitur 8057 digabungkan. Klik Tutup .
- Anda akan melihat layer baru california_total_populationdimuat di panel Layers . Pada titik ini, bidang dari file CSV digabungkan dengan lapisan saluran sensus. Sekarang setelah kita memiliki data kependudukan di lapisan sensus, kita dapat menatanya untuk membuat visualisasi distribusi kepadatan penduduk. Klik tombol Buka Panel Penataan Lapisan .
- Di panel Layer Styling , pilih Graduateddari menu drop-down. Saat kami ingin membuat peta kepadatan populasi, kami ingin menetapkan warna berbeda untuk setiap fitur saluran sensus berdasarkan kepadatan populasi. Kami memiliki populasi di kolom Estimasi!!Total!!Total populasi , dan area area di ALAND . Klik tombol Ekspresi , untuk menghitung persentase jumlah penduduk pada setiap saluran pencacahan.
- Masukkan ekspresi berikut untuk menghitung kepadatan populasi. Area fitur diberikan dalam kilometer persegi. Kami kemudian mengubahnya menjadi meter persegi dengan mengalikan dengan 1000000dan menghitung kepadatan penduduk dengan rumus Penduduk/Luas . Pratinjau hasilnya dan klik OK .
- Di Panel Penataan Lapisan , klik klasifikasikan dan masukkan kelas sebagai 10.
- Klik pada jalur warna untuk memilih jalur warna RdYlGn.
- Kerapatan yang lebih tinggi lebih penting, mari kita tetapkan warna hijau untuk kerapatan rendah dan merah untuk area dengan kerapatan tinggi. Klik pada jalur warna dan pilih Balikkan Jalur Warna .
- Sekarang kami memiliki visualisasi informasi kepadatan populasi yang sangat baik di California. Untuk membuatnya lebih baik, mari kita buat batas setiap saluran sensus transparan. Klik pada tab Simbol.
- Klik pada warna Stroke dan klik .Transparent stroke
- Tempat sampah dapat disesuaikan, klik pada Nilai ini akan memunculkan dialog untuk memasukkan nilai batas atas dan bawah.
- Setelah Anda puas menutup panel styling Layer. Kami sekarang memiliki visualisasi informasi kepadatan populasi yang terlihat bagus di California.

2. Performing Spatial Joins (QGIS3)
- Temukan nybb_19a.zipfile di Peramban QGIS dan kembangkan. Pilih nybb_19a/nybb.shplayer dan seret ke kanvas. Ini adalah lapisan poligon yang mewakili batas wilayah di kota New York.
- Selanjutnya, cari V_SSS_SEGMENTRATING_1.zipfile dan perluas. Pilih dot_V_SSS_SEGMENTRATING_1_20190129.shplayer dan tambahkan ke kanvas. Ini adalah lapisan garis dari semua jalan di kota.
- Mari kita periksa atribut yang tersedia untuk setiap fitur dot_V_SSS_SEGMENTRATING_1_20190129layer. Klik kanan dan pilih Open Attribute Table .

3. Performing Spatial Queries (QGIS3)
- Locate the metro_stations_accessbility.zip file in the QGIS Browser and expand it. Select the metro_stations_accessbility.shp file and drag it to the canvas. A new layer metro_stations_accessbility will be loaded in the Layers panel.
- The data layer for bars and pubs is in the CSV format. To load it in QGIS, go to Layer ‣ Add Layer ‣ Add Delimited Text Layer…. ( See Importing Spreadsheets or CSV files (QGIS3) for more details on importing CSV files)
- In the Data Source Manager | Delimited Text dialog, browse and select the downloaded Bars_and_pubs__with_patron_capacity.csv file as File name. The X field and Y field columns should be auto selected to x coordinate and y coordinate respectively. Click Add.

4. Nearest Neighbor Analysis (QGIS3)
- Temukan ne_10m_populated_places_simple.zipfile yang diunduh di panel Browser dan perluas. Seret ne_10m_populated_places_simple.shpfile ke kanvas.
- Anda akan melihat layer baru ne_10m_populated_places_simpledimuat di panel Layers . Lapisan ini berisi titik-titik yang mewakili tempat berpenduduk. Sekarang kita akan memuat lapisan gempa bumi. Lapisan ini hadir sebagai file teks Tab Serepated Values ​​(TSV) . Untuk memuat file ini, klik tombol Open Data Source Manager di Data Source Toolbar . Anda juga dapat menggunakan pintasan keyboard.Ctrl + L
- Di kotak dialog Pengelola Sumber Data , pilih Teks yang Dibatasi .

5. Sampling Raster Data using Points or Polygons (QGIS3)
- Buka zip dan ekstrak keduanya 2018_Gaz_ua_national.zipdan tl_2018_us_county.zipke folder di komputer Anda. Buka QGIS dan cari us.tmax_nohads_ll_20190501_float.tiffile di Peramban QGIS seret ke kanvas.
- Anda akan melihat layer raster baru us.tmax_nohads_ll_20190501_floatdimuat di panel Layers . Lapisan raster ini berisi suhu maksimum yang tercatat pada setiap piksel dalam derajat Celcius. Selanjutnya kita akan memuat file titik area perkotaan. File ini hadir sebagai file teks dalam format Tab Separated Values ​​(TSV). Klik tombol Buka Pengelola Sumber Data di Bilah Alat Sumber Data .
- Beralih ke tab Teks Dibatasi . Klik tombol … di sebelah Nama file dan tentukan jalur ke file teks yang Anda unduh. Di bagian File format , pilih Custom delimiters dan centang Tab . Pilih INTPTLONGsebagai bidang X dan INTPTLATsebagai bidang Y . Klik Tambah lalu Tutup .

6. Calculating Raster Area (QGIS3)
- Buka zip semua file yang diunduh. Di Browser , cari folder yang berisi file batas WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons.shpdan seret dan lepas ke kanvas QGIS.
- Sekarang cari ubin raster penutup dunia ESA_WorldCover_10m_2020_v100_N24_E093_Map.tifdan jatuhkan ke kanvas QGIS.
- Anda sekarang akan memiliki layer vektor batas dan raster tutupan lahan dimuat di panel Layers . Mari potong raster tutupan lahan ke batas taman nasional. Pergi ke Processing ‣ Toolbox untuk membuka Processing toolbox. Cari dan temukan GDAL ‣ Ekstraksi raster ‣ Klip raster dengan algoritma lapisan topeng. Klik dua kali untuk meluncurkannya.

7. Creating Heatmaps (QGIS3)
- Pertama-tama kita akan memuat layer peta dasar dari OpenStreetMap dan kemudian mengimpor data CSV. Di tab Browser , gulir ke bawah dan temukan bagian Ubin XYZ .
- Bentangkan untuk melihat layer petak OpenStreetMap . Seret dan lepas ke kanvas utama. Selanjutnya kita akan memuat file CSV. Klik tombol Buka Pengelola Sumber Data .
- Beralih ke tab Teks Dibatasi . Di sini kita akan mengimpor data kejahatan yang datang dalam file teks format CSV. Klik tombol … di sebelah Nama file dan ramban ke 2019-02-surrey-street.csvfile yang diunduh. Bidang X dan bidang Y di bagian Definisi Geometri akan diisi secara otomatis dengan kolom Longitudedan Latitude. Geometri CRS harus dibiarkan definisi default . Pastikan data terlihat benar di panel Sample data dan klik Add , diikuti oleh Close .EPSG:4326 - WGS 84

8. Animating Time Series Data (QGIS3)
- Di Panel Peramban QGIS, temukan direktori tempat Anda menyimpan data unduhan. Luaskan ne_10m_land.zipdan pilih ne_10m_land.shplayer. Seret layer ke kanvas. Selanjutnya, cari ASAM_shp.zipfile. Perluas dan pilih asam_data_download/ASAM_events.shplayer dan seret ke kanvas.
- Setelah lapisan dimuat, Anda dapat melihat masing-masing titik yang mewakili insiden lokasi pembajakan. Ada ribuan insiden dan sulit ditentukan dengan lebih banyak pembajakan. Daripada poin individual, cara yang lebih baik untuk memvisualisasikan data ini adalah melalui peta panas. Pilih ASAM_eventslayer dan klik tombol Open the layer Styling Panel di panel Layers . Klik tarik-turun.Single symbol
- Di drop-down pemilihan penyaji, pilih Heatmappenyaji. Selanjutnya, pilih jalur Viridiswarna dari pemilih Jalur warna .

9. Handling Invalid Geometries (QGIS3)
- Jelajahi India-States.zipfile yang diunduh di Peramban QGIS. Perluas dan seret India-States.shpfile ke kanvas peta.
- Anda akan melihat India-Stateslayer baru dimuat di panel Layers . Pergi ke Memproses ‣ Kotak Alat .
- Kami akan mencoba menjalankan algoritme pemrosesan pada lapisan input untuk mendemonstrasikan bagaimana geometri yang tidak valid dapat menyebabkan masalah selama operasi geoproses. Cari dan temukan Kartografi ‣ Algoritma pewarnaan topologi. Klik dua kali untuk meluncurkannya.

1. Masalah Utama SQL Server di Docker: Hostname & Resolusi DNS

Di dalam Docker, container biasanya berkomunikasi menggunakan nama container atau IP internal Docker (bukan nama hostname fisik komputer Anda). Replikasi SQL Server sangat bergantung pada nama server asli (server name).

    Pastikan Nama Server Sesuai (@@SERVERNAME):
    Jalankan query ini di masing-masing container untuk memastikan nama server di SQL Server sesuai dengan nama container-nya:
    SQL

    SELECT @@SERVERNAME;

    Jika nama server tidak sesuai dengan nama host container, Anda harus memperbaikinya dengan script berikut:
    SQL

    EXEC sp_dropserver 'NAMA_LAMA';
    EXEC sp_addserver 'NAMA_BARU', 'local';

    (Catatan: Setelah menjalankan perintah ini, Anda harus me-restart container SQL Server).

    Konfigurasi Koneksi Antar-Container (Docker Network):
    Pastikan kedua container Docker berada dalam Docker Network yang sama (misalnya bridge network buatan Anda sendiri atau default network), sehingga mereka bisa saling ping menggunakan nama container (contoh: ping sql_primary_server).


    -- Hapus nama server lama yang berupa ID acak Docker
EXEC sp_dropserver '3ed780506897';
GO

-- Tambahkan nama server baru (sesuaikan dengan nama yang ingin Anda gunakan, misal 'sql_primary_server')
EXEC sp_addserver 'sql_primary_server', 'local';
GO




-- Hapus nama server lama
EXEC sp_dropserver 'bf8aba325d64';
GO

-- Tambahkan nama server baru
EXEC sp_addserver 'sql_secondary_server', 'local';
GO
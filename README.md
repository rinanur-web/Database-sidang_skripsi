# Sistem Informasi Penjadwalan Sidang Skripsi
# Membuat Entity Relationship Diagram(ERD)
Berikut ERD dari sistem ini:
![ERD SI SIDANG SKRIPSI OTOMATIS drawio (1)](https://github.com/user-attachments/assets/9ee82b1a-b86b-4320-8cc8-1153cca52e14)
# Membuat relasi tabel di MySQL
1. Membuat database dengan nama db_sidangskripsi
2. Membuat tabel sesuai dengan erd yang telah dibuat
   
   a. CREATE TABLE mahasiswa - Menyimpan data mahasiswa
   
         CREATE TABLE mahasiswa (
         id_mhs int NOT NULL AUTO_INCREMENT,
         nama_mhs varchar(50) NOT NULL,
         nim int NOT NULL,
         prodi_mhs enum('D3 TI','D4 RKS','D4 ALKS','D4 TRM','D4 RPL')NOT NULL,
         thn_akademik varchar(25) NOT NULL,
         judul_skripsi varchar(100) NOT NULL,
         id_user int NOT NULL,
         PRIMARY KEY (id_mhs),
         KEY id_user (id_user),
         CONSTRAINT FK_mahasiswa_user FOREIGN KEY (id_user) REFERENCES user (id_user) ON DELETE CASCADE ON UPDATE CASCADE);
   
   b. CREATE TABLE dosen - Menyimpan data dosen
   
         CREATE TABLE dosen (
         id_dosen int NOT NULL AUTO_INCREMENT,
         nama_dosen varchar(50) NOT NULL,
         nip int NOT NULL,
         id_user int DEFAULT NULL,
         PRIMARY KEY (id_dosen),
         KEY id_user (id_user),
         CONSTRAINT FK_dosen_user FOREIGN KEY (id_user) REFERENCES user (id_user) ON DELETE CASCADE ON UPDATE CASCADE);
   
   c. CREATE TABLE dosen penguji - Menyimpan data dosen penguji yang ditugaskan untuk suatu sidang
   
      Pada tabel dosen_penguji terdapat unique key pada kombinasi kolom id_sidang dan id_dosen. Unique key ini dibuat untuk mencegah duplikasi data, sehingga satu dosen penguji tidak dapat terdaftar lebih dari satu kali dalam sidang yang sama.

         CREATE TABLE dosen_penguji(
         id_penguji int NOT NULL AUTO_INCREMENT,
         id_sidang int NOT NULL,
         id_dosen int NOT NULL,
         peran enum('PENGUJI 1','PENGUJI 2') NOT NULL,
         PRIMARY KEY (id_penguji),
         UNIQUE KEY unique_sidang_dosen (id_sidang,id_dosen),
         KEY fk_dosen(id_dosen),
         CONSTRAINT fk_dosen FOREIGN KEY (id_dosen) REFERENCES dosen (id_dosen) ON DELETE CASCADE ON UPDATE CASCADE,
         CONSTRAINT fk_sidang FOREIGN KEY (id_sidang) REFERENCES sidang (id_sidang) ON DELETE CASCADE ON UPDATE CASCADE
         );

   d. CREATE TABLE ruangan - Menyimpan data ruangan yang tersedia untuk pelaksanaan sidang

         CREATE TABLE ruangan (
         id_ruangan int NOT NULL AUTO_INCREMENT,
         kode_ruangan varchar(25) NOT NULL,
         nama_ruangan varchar(50) NOT NULL,
         PRIMARY KEY (id_ruangan));

   e. CREATE TABLE sidang - Menyimpan data jadwal sidang skripsi untuk tiap mahasiswa

         CREATE TABLE sidang (
         id_sidang int NOT NULL AUTO_INCREMENT,
         id_mhs int NOT NULL,
         id_ruangan int NOT NULL,
         tanggal_sidang date NOT NULL,
         waktu_mulai time NOT NULL,
         waktu_selesai time NOT NULL,
         status enum('DITUNDA','DIJADWALKAN','DIBATALKAN')NOT NULL,
         PRIMARY KEY (id_sidang),
         KEY fk_mhs (id_mhs),
         KEY fk_ruangan (id_ruangan),
         CONSTRAINT fk_mhs FOREIGN KEY (id_mhs) REFERENCES mahasiswa (id_mhs) ON DELETE CASCADE ON UPDATE CASCADE,
         CONSTRAINT fk_ruangan FOREIGN KEY (id_ruangan) REFERENCES ruangan (id_ruangan) ON DELETE CASCADE ON UPDATE CASCADE);

   f. CREATE TABLE user - Menyimpan akun login

         CREATE TABLE user (
         id_user int NOT NULL AUTO_INCREMENT,
         username varchar(50) NOT NULL,
         password varchar(255) NOT NULL,
         role enum(mahasiswa,dosen) NOT NULL DEFAULT mahasiswa,
         isAdmin enum('Y','N') DEFAULT 'N',
         PRIMARY KEY (id_user));

   g. CREATE TABLE user notification - Menyimpan notifikasi yang dikirim ke user di dalam sistem

         CREATE TABLE user_notifications (
         id int unsigned NOT NULL AUTO_INCREMENT,
         user_id int unsigned NOT NULL,
         message text NOT NULL,
         status enum(‘unread','read') NOT NULL DEFAULT 'unread',
         created_at datetime DEFAULT NULL,
         updated_at datetime DEFAULT NULL,
         PRIMARY KEY (id));
   
4. Membuat query INSERT untuk menambahkan data ke dalam tabel yang sudah dibuat

   a. INSERT mahasiswa

         INSERT INTO mahasiswa VALUES 
         (1, 'esna', 1234, 'D3 TI', '2024/2025', 'lkgskuhgui',23),
         (3, 'wahyu', 812345, 'D3 TI', '2024/2025', 'jhjwyfy',24),
   
   b. INSERT dosen

         INSERT INTO dosen VALUES 
         (2, 'novi', 56362,44),
         (3, 'wahyu', 812345,45);

   c. INSERT dosen penguji

         INSERT INTO dosen_penguji VALUES 
         (15, 6, 3, 'PENGUJI 1'),(16, 6, 2, 'PENGUJI 2'),(17, 7, 2, 'PENGUJI 1');

   d. INSERT ruangan

         INSERT INTO ruangan VALUES
         (1, 'J.5.4', 'Lab Sistem Informasi'),(2, 'J.4.5', 'Lab Jaringan');

   e. INSERT sidang

          INSERT INTO sidang VALUES
          (6, 1, 2, '2025-02-25', '14:23:20', '14:23:23', 'DIJADWALKAN'),
          (7, 3, 2, '2025-02-27', '14:23:52', '14:23:58', 'DIJADWALKAN');

   f. INSERT user

         INSERT INTO `user` (`id_user`, `username`, `password`, `role`, `isAdmin`) VALUES
         (43, 'admin','$2y$12$O1L8NoO9N7YnVduIHFz02eJwulvWK1gEmAGkdVy5tZNzzi1S2uvdK', 'mahasiswa', 'Y'),
         (44, 'Dosen 1', '$2y$12$sJ37rO5HoSp3yFhZ6a/2G.HGyZOQBdOpB6tIXbCD9YqFn6WsPeNBW', 'dosen', 'Y'),
         (45, 'Dosen 2', '$2y$12$5.FIhvr7lVKDUbQEYWfzYO7FMPIfhYHA7SaUPJHSSwdqsx48/7qba', 'dosen', 'N'),
         (48, 'Dendi Ardiansyah', '$2y$12$AevIpK7R86Qr5sg2bwFwH.NpUHJM2nSmJkco.zVYQ.CYMTRo5MQp.', 'mahasiswa', 'N');

   g. INSERT user notification

         INSERT INTO `user_notifications` (`id`, `user_id`, `message`, `status`, `created_at`, `updated_at`) VALUES
         (1, 22, 'Sidang mahasiswa dengan ID 77 telah dijadwalkan pada 2026-01-01 pukul 08:00:00 - 09:00:00 di ruang Lab Teknologi Jaringan.',
         'unread', '2025-05-15 15:40:23',    '2025-05-15 15:40:23');

5. Membuat query JOIN yang digunakan untuk menampilkan gabungan data dari beberapa tabel

   a. JOIN jadwal mahasiswa
   
      Join jadwal mahasiswa ini untuk menampilkan jadwal sidang mahasiswa → berisi data mahasiswa + tanggal & waktu sidang + ruangan sidang.

         SELECT
      	a.nama_mhs,a.prodi_mhs,a.judul_skripsi,b.tanggal_sidang, 
      	b.waktu_mulai,c.kode_ruangan,c.nama_ruangan
         FROM
      	mahasiswa AS a INNER JOIN sidang AS b
         ON a.id_mhs = b.id_mhs
      	INNER JOIN ruangan AS c ON b.id_ruangan = c.id_ruangan

   b. JOIN jadwal dosen
   
      Join jadwal dosen digunakan untuk menampilkan jadwal sidang beserta peran dosen penguji (Penguji 1 dan Penguji 2) untuk setiap mahasiswa yang akan sidang

         SELECT
      	d.nama_mhs, c.tanggal_sidang, c.waktu_mulai, 
      	MAX(CASE WHEN b.peran = 'PENGUJI 1' 
         THEN a.nama_dosen END) AS penguji_1, 
      	MAX(CASE WHEN b.peran = 'PENGUJI 2'
       	THEN a.nama_dosen END) AS penguji_2, 
      	e.kode_ruangan, e.nama_ruangan
         FROM
      	mahasiswa AS d  JOIN sidang AS c ON d.id_mhs = c.id_mhs
      	JOIN dosen_penguji AS b ON c.id_sidang = b.id_sidang
      	JOIN dosen AS a ON b.id_dosen = a.id_dosen
      	JOIN ruangan AS e ON c.id_ruangan = e.id_ruangan
         GROUP BY
      	d.nama_mhs, c.tanggal_sidang, c.waktu_mulai, 
         e.kode_ruangan, e.nama_ruangan

   c. JOIN detail sidang
   
      Join detail sidang digunakan untuk menampilkan detail lengkap sidang → data mahasiswa + penguji + tanggal & waktu + ruangan + status sidang.

         SELECT
      	b.nama_mhs, b.nim, b.thn_akademik, b.prodi_mhs, 
      	MAX(CASE WHEN c.peran = 'PENGUJI 1' THEN d.nama_dosen
       	END) AS penguji_1, 
      	MAX(CASE WHEN c.peran = 'PENGUJI 2' THEN d.nama_dosen
       	END) AS penguji_2, 
      	a.tanggal_sidang, a.waktu_mulai, a.waktu_selesai, 
      	e.kode_ruangan, e.nama_ruangan, a.`status`
         FROM
      	sidang AS a INNER JOIN mahasiswa AS b
      	ON a.id_mhs = b.id_mhs
      	INNER JOIN dosen_penguji AS c
      	ON a.id_sidang = c.id_sidang
      	INNER JOIN dosen AS d
      	ON c.id_dosen = d.id_dosen
      	INNER JOIN ruangan AS e
      	ON a.id_ruangan = e.id_ruangan
         GROUP BY
      	a.id_sidang;

  



      









   
   



   


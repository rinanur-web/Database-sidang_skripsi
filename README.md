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


   
   



   


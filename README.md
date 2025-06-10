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

   


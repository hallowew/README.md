# Dokumentasi
Microservices adalah pendekatan arsitektur yang semakin populer dalam pengembangan perangkat lunak modern. Dengan memecah aplikasi menjadi layanan-layanan kecil yang independen, pengembang dapat mencapai skala, ketahanan, dan penyebaran yang lebih baik. Selain itu, penggunaan REST API menjadi semakin umum sebagai cara untuk mengintegrasikan berbagai layanan tersebut.
Salah satu implementasi praktis dari arsitektur microservices dapat dilakukan dengan menggunakan Flask, sebuah framework web Python yang ringan dan mudah digunakan, serta PostgreSQL sebagai sistem manajemen basis data untuk menyimpan dan mengelola data aplikasi. Selain itu, keamanan data pengguna menjadi hal yang sangat krusial. Oleh karena itu, penggunaan enkripsi menjadi suatu keharusan untuk melindungi kerahasiaan dan integritas data, terutama pada informasi rahasia seperti kata sandi pengguna.

Kelompok 4:
- Dandi Gustiawan | 220401010115
- Carthy Arung | 220401010126
- Dara Nailah Ikhsyan | 220401010091

## Laporan Aplikasi
Berikut Laporan aplikasi dengan fungsi CRUD untuk Login dan Manajemen Akses User dengan enkripsi data menggunakan AES :

### Inisialisasi Library

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from cryptography.fernet import Fernet
```
Script diatas merupakan inisialisasi dasar atau langkah awal untuk membuat aplikasi web menggunakan Flask dengan integrasi SQLAlchemy untuk berinteraksi dengan database serta menggunakan cryptography library (fernet) untuk implementasi enkripsi dan dekripsi.
Berikut penjelasannya :
- ```flask``` merupakan library utama (Python) untuk membuat aplikasi web menggunakan framework Flask.
- ```request``` digunakan untuk mengelola data yang diterima dari permintaan HTTP.
- ```Jsonify``` merupakan fungsi untuk mengubah objek Python menjadi respons JSON.
- ```SQLAlchemy``` merupakan library ORM (Object-Relational Mapping) untuk berinteraksi dengan database.
- ```Cryptography.fernet``` merupakan modul dari library cryptography yang menyediakan implementasi enkripsi dengan Fernet symmetric key.

Script tersebut digunakan untuk membangun aplikasi web menggunakan Flask dan SQLAlchemy. Kemudian, dapat dilanjutkan dengan menambahkan rute (routes), model database, dan fungsi lain sesuai kebutuhan aplikasi.

### Inisialisasi App
```python
app = Flask(_name_)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:root@127.0.0.1/db_uts'
app.config['SQLALCHEMY_TRACK_MODIFICATION'] = False

db = SQLAlchemy(app)
```
Script diatas merupakan langkah-langkah awal dalam mengonfigurasi dan menginisialisasi sebuah aplikasi Flask yang akan digunakan untuk membuat RESTful API atau aplikasi web. Berikut penjelasannya :
- ```app = Flask(__name__)``` digunakan untuk membuat instance dari Flask yang akan menjadi dasar dari aplikasi web. mengatur konfigurasi database PostgreSQL, dan inisialisasi objek SQLAlchemy. Parameter __name__ memberikan Flask informasi tentang lokasi modul aplikasi.
- ```app.config['SQLALCHEMY_DATABASE_URI']``` yaitu menentukan URL koneksi ke database PostgreSQL yang akan digunakan oleh SQLAlchemy. Dalam hal ini, koneksi ke database "db_uts" dengan nama pengguna "postgres" dan kata sandi "root".
- ```app.config['SQLALCHEMY_TRACK_MODIFICATION']``` yaitu menonaktifkan fitur pemantauan atau pelacakan perubahan oleh SQLAlchemy, serta mengonfigurasi agar SQLAlchemy tidak melacak perubahan secara otomatis.
- ```db = SQLAlchemy(app)``` digunakan untuk membuat objek SQLAlchemy untuk berinteraksi dengan database menggunakan ORM (Object-Relational Mapping) yang dimana akan mewakili struktur database dan memberikan antarmuka untuk berinteraksi dengan database.

Penggunaan script tersebut berfokus pada konfigurasi awal aplikasi dan menyediakan alat untuk berinteraksi dengan database PostgreSQL menggunakan SQLAlchemy

### Inisialisasi Encrypt AES
```python
key = Fernet.generate_key()
cipher_suite = Fernet(key)
```
Script diatas merupakan bagian dari inisialisasi enkripsi menggunakan algoritma AES (Advanced Encryption Standard) dengan Fernet dari library cryptography.
Berikut penjelasannya :
- ```key = Fernet.generate_key()``` digunakan untuk membuat kunci enkripsi baru menggunakan fungsi generate_key() dari kelas Fernet untuk mengamankan proses enkripsi dan dekripsi.
- ```cipher_suite = Fernet(key)``` digunakan untuk membuat objek Fernet yang akan digunakan untuk melakukan enkripsi dan dekripsi yang diinisialisasi dengan kunci yang telah dihasilkan sebelumnya.

Script tersebut menggunakan Fernet yang menyiratkan penggunaan enkripsi simetris, di mana kunci yang sama digunakan untuk enkripsi dan dekripsi, kunci tersebut juga harus dijaga secara rahasia. Dengan objek *cipher_suite*, selanjutnya dapat digunakan metode seperti *encrypt()* dan *decrypt()* untuk melakukan enkripsi dan dekripsi teks.

### Defined Model User
```python
class User(db.Model):
    _tablename_ = 'users'
    id = db.Column (db.Integer,primary_key=True)
    username = db.Column(db.String(50),unique=True, nullable=False)
    password = db.Column(db.String(500), nullable=False)
```
Script diatas merupakan definisi model data untuk tabel User dalam konteks ORM (Object-Relational Mapping) menggunakan SQLAlchemy dalam aplikasi Flask.
Berikut penjelasannya :
- ```class User(db.Model)``` digunakan untuk membuat kelas Python User yang merupakan turunan dari kelas db.Model, yang dimana User adalah model data yang akan direpresentasikan dalam bentuk tabel di dalam database.
- ```__tablename__ = 'users'``` yaitu penetapan nama tabel database menjadi 'users' yang merupakan properti yang digunakan oleh SQLAlchemy untuk menentukan nama tabel yang sesuai dengan model.
- ```id = db.Column(db.Integer, primary_key=True)``` digunakan untuk mendefinisikan kolom id sebagai kolom bertipe integer yang merupakan kunci utama (primary key) tabel. 
- ```username = db.Column(db.String(50), unique=True, nullable=False)``` digunakan untuk mendefinisikan kolom username sebagai kolom bertipe string dengan batasan panjang 50 karakter, bersifat unik, dan tidak dapat bernilai null.
- ```password = db.Column(db.String(500), nullable=False)``` digunakan untuk mendefinisikan kolom password sebagai kolom bertipe string dengan batasan panjang 500 karakter dan tidak dapat bernilai null.

Script tersebut digunakan untuk mendefinisikan struktur tabel User yang akan digunakan dalam database dimana setiap objek User akan direpresentasikan sebagai baris dalam tabel users. Properti-properti dalam kelas tersebut mencerminkan kolom-kolom dalam tabel, dan batasan yang ditetapkan pada properti tersebut akan diterjemahkan ke dalam batasan pada level database. Setelah definisi model tersebut, selanjutnya dapat menggunakan SQLAlchemy untuk membuat atau melakukan query terhadap tabel users yang sesuai dengan model User.

## API
### Get All User
```python
@app.route('/api/users', methods=['GET'])
def get():
    users = User.query.all();
    data = []
    for user in users:
        
        data.append({'username': user.username, 'password' : user.password})
    
    return jsonify({'users' : data})
```
Script diatas adalah bagian dari aplikasi web Flask yang mengimplementasikan sebuah endpoint API yang digunakan untuk mengambil (GET) data semua pengguna dari tabel users dalam database. Berikut penjelasannya :
- ```@app.route('/api/users', methods=['GET'])``` yaitu mendekorasi fungsi get() sebagai endpoint *‘/api/users’* yang mendukung metode HTTP GET dimana endpoint ini akan menanggapi permintaan GET pada URL /api/users.
- ```def get()``` yaitu mendefinisikan fungsi get() yang akan dijalankan ketika permintaan GET diterima pada endpoint */api/users*.
- ```users = User.query.all()``` yaitu mengambil semua data pengguna dari tabel users menggunakan metode *query.all()* dari model User.
- ```data = []``` digunakan untuk membuat daftar kosong untuk menyimpan data pengguna yang akan dikembalikan dalam respons.
- ```for user in users: data.append({'username': user.username, 'password' : user.password})``` yaitu mengiterasi melalui setiap objek pengguna yang ditemukan dan menambahkannya ke daftar data dalam bentuk dictionary yang berisi informasi username dan password.
- ```return jsonify({'users' : data})``` yaitu mengonversi daftar data menjadi format JSON dan mengembalikannya sebagai respons dengan menggunakan fungsi jsonify dari Flask, yang secara otomatis mengelola konversi objek Python menjadi format JSON yang benar.

Script tersebut digunakan untuk membuat endpoint API yang mengembalikan data semua pengguna dari tabel users dalam format JSON ketika ada permintaan GET pada URL */api/users*. Dengan menggunakan SQLAlchemy, data dari tabel users diambil dan diubah ke dalam format yang sesuai sebelum dikirimkan sebagai respons. Endpoint tersebut dapat diakses melalui browser atau oleh aplikasi klien lainnya yang dapat membuat permintaan HTTP GET ke URL tersebut untuk mengambil data pengguna.

### Create New User
```python
@app.route('/api/users', methods=['POST'])
def create() :
    data = request.get_json()
    user = User(id=data['id'], username=data['username'], password=cipher_suite.encrypt(data['password'].encode()).decode())
    db.session.add(user)
    db.session.commit()

    return jsonify({'message' : 'User Successfully created'}), 201
```

Script ini adalah bagian dari aplikasi web Flask yang mengimplementasikan endpoint API untuk membuat (POST) pengguna baru dan menyimpannya ke dalam database dengan menggunakan enkripsi. Berikut penjelasannya :
- ```@app.route('/api/users', methods=['POST'])``` yaitu mendekorasi fungsi create() sebagai endpoint *‘/api/users’* yang mendukung metode HTTP POST yang dimana endpoint ini akan menanggapi permintaan POST pada URL *‘/api/users’*.
- ```def create():``` yaitu mendefinisikan fungsi create() yang akan dijalankan ketika permintaan POST diterima pada endpoint *‘/api/users’*.
- ```data = request.get_json()``` yaitu mengambil data JSON dari permintaan POST menggunakan fungsi get_json() dari objek request
- ```user = User(id=data['id'], username=data['username'], password=cipher_suite.encrypt(data['password'].encode()).decode())``` yaitu membuat objek User baru dengan menggunakan data yang diterima dari permintaan. 
- ```db.session.add(user)``` yaitu menambahkan objek user untuk disimpan dalam database pada transaksi selanjutnya.
- ```db.session.commit()``` yaitu melakukan commit transaksi, menyimpan objek user ke dalam database secara permanen.
- ```return jsonify({'message' : 'User Successfully created'}), 201``` yaitu mengembalikan respons JSON yang menyatakan bahwa pengguna telah berhasil dibuat. 

Script tersebut digunakan untuk membuat endpoint API untuk membuat pengguna baru dengan mengirimkan data JSON melalui permintaan POST ke URL *‘/api/users’* dimana data pengguna yang diterima dari klien digunakan untuk membuat objek User, dan passwordnya dienkripsi sebelum disimpan ke dalam database. Setelah disimpan ke database, respons JSON dikirimkan kembali ke klien untuk memberi tahu bahwa pengguna telah berhasil dibuat.

### Get Detail User
```python
@app.route('/api/users/<id>', methods=['GET'])
def show(id):
    user = User.query.filter_by(id=id).first()
    if user:
        return jsonify({"id" : user.id, "username" : user.username, "password" : user.password }), 200
    
    return jsonify({"message" : "User not found"}), 404
```

Script diatas adalah bagian dari aplikasi web Flask yang mengimplementasikan sebuah endpoint API untuk mendapatkan (GET) detail pengguna berdasarkan ID dari database.
Berikut penjelasannya :
- ```@app.route('/api/users/<id>', methods=['GET'])``` yaitu endekorasi fungsi show() sebagai endpoint *‘/api/users/<id>’* yang mendukung metode HTTP GET. Parameter <id> adalah variabel yang akan menyimpan nilai ID pengguna yang diberikan dalam URL dimana endpoint ini akan menanggapi permintaan GET pada URL *‘/api/users/<id>’*.
- ```def show(id)``` yaitu mendefinisikan fungsi show() yang akan dijalankan ketika permintaan GET diterima pada endpoint *‘/api/users/<id>’* untuk mengambil parameter id yang sesuai dengan nilai ID yang diberikan dalam URL.
- ```user = User.query.filter_by(id=id).first()``` yaitu mencari pengguna dalam database berdasarkan ID yang diberikan. Fungsi *filter_by()* digunakan untuk memfilter data berdasarkan kriteria yang diberikan, dan *first()* mengembalikan objek pertama yang sesuai atau None jika tidak ada yang ditemukan.
- ```return jsonify({"id" : user.id, "username" : user.username, "password" : user.password }), 200``` yaitu mengembalikan detail pengguna dalam format JSON bersama dengan kode status HTTP 200 (OK) jika pengguna ditemukan.
- ```return jsonify({"message" : "User not found"}), 404``` yaitu mengembalikan pesan bahwa pengguna tidak ditemukan dalam format JSON bersama dengan kode status HTTP 404 (Not Found) jika pengguna tidak ditemukan.

Script tersebut digunakan untuk membuat endpoint API untuk mendapatkan detail pengguna berdasarkan ID dari database. Saat menerima permintaan GET dengan ID pengguna tertentu, fungsi show() mencari pengguna dalam database. Jika ditemukan, detail pengguna dikembalikan sebagai respons JSON. Jika tidak ditemukan, respons JSON dengan pesan "User not found" dan kode status HTTP 404 dikirimkan kembali ke klien.

### Update Data User
```python
@app.route('/api/users/<id>', methods=['PUT'])
def update(id):
    user = User.query.filter_by(id=id).first()
    if user:
        data = request.get_json()
        user.username = data['username']
        user.password = cipher_suite.encrypt(data['password'].encode()).decode()
        db.session.commit()
        return jsonify({"message" : "User Updated Successfully"})
    
    return jsonify({"message" : "User not found"}), 404
```
Script diatas adalah bagian dari aplikasi web Flask yang mengimplementasikan sebuah endpoint API untuk memperbarui (PUT) data pengguna berdasarkan ID dalam database.
Berikut penjelasannya :
- ```@app.route('/api/users/<id>', methods=['PUT'])``` yaitu mendekorasi fungsi update() sebagai endpoint *‘/api/users/<id>’* yang mendukung metode HTTP PUT. Parameter <id> merupakan variabel yang akan menyimpan nilai ID pengguna yang diberikan dalam URL dimana endpoint ini akan menanggapi permintaan PUT pada URL *‘/api/users/<id>’*.
- ```def update(id)``` yaitu mendefinisikan fungsi update() yang akan dijalankan ketika permintaan PUT diterima pada endpoint *‘/api/users/<id>’* untuk mengambil parameter id yang sesuai dengan nilai ID yang diberikan dalam URL.
- ```user = User.query.filter_by(id=id).first()``` yaitu mencari pengguna dalam database berdasarkan ID yang diberikan. Jika ditemukan, objek pengguna disimpan dalam variabel user.
- ```data = request.get_json()``` yaitu mengambil data JSON dari tubuh permintaan (request body) menggunakan fungsi get_json() dari objek request.
- ```user.username = data['username']``` yaitu memperbarui nilai kolom username pengguna dengan nilai yang baru dari data JSON.
- ```user.password = cipher_suite.encrypt(data['password'].encode()).decode()``` yaitu memperbarui nilai kolom password pengguna dengan nilai yang baru dari data JSON setelah dienkripsi.
- ```db.session.commit()``` yaitu melakukan commit perubahan ke dalam database.
- ```return jsonify({"message" : "User Updated Successfully"})``` yaitu mengembalikan respons JSON yang menyatakan bahwa pengguna telah berhasil diperbarui.
- ```return jsonify({"message" : "User not found"}), 404``` yaitu mengembalikan respons JSON dengan pesan "User not found" dan kode status HTTP 404 jika pengguna tidak ditemukan.

Script tersebut digunakan untuk membuat endpoint API untuk memperbarui data pengguna berdasarkan ID dari database. Saat menerima permintaan PUT dengan ID pengguna tertentu, fungsi update() mencari pengguna dalam database. Jika ditemukan, data pengguna diperbarui berdasarkan data JSON yang diterima dari klien, dan respons JSON yang menyatakan bahwa pengguna telah berhasil diperbarui dikirimkan kembali ke klien. Jika pengguna tidak ditemukan, respons JSON dengan pesan "User not found" dan kode status HTTP 404 dikirimkan kembali ke klien

### Delete
```python
@app.route('/api/users/<id>', methods=['DELETE'])
def destroy(id):    
    user = User.query.filter_by(id=id).first()
    if user:
        user = User.query.filter_by(id=id).first()
        db.session.delete(user)
        db.session.commit()
        return jsonify({"message":"User deleted successfully"})
    return jsonify({"message":"User not found"}), 404
```
Script diatas adalah bagian dari aplikasi web Flask yang mengimplementasikan sebuah endpoint API untuk menghapus (DELETE) pengguna berdasarkan ID dari database.
Berikut penjelasannya :
- ```@app.route('/api/users/<id>', methods=['DELETE'])``` yaitu mendekorasi fungsi destroy() sebagai endpoint *‘/api/users/<id>’* yang mendukung metode HTTP DELETE. Parameter <id> adalah variabel yang akan menyimpan nilai ID pengguna yang diberikan dalam URL dimana endpoint ini akan menanggapi permintaan DELETE pada URL *‘/api/users/<id>’*.
- ```def destroy(id)``` yaitu mendefinisikan fungsi destroy() yang akan dijalankan ketika permintaan DELETE diterima pada endpoint *‘/api/users/<id>’* untuk mengambil parameter id yang sesuai dengan nilai ID yang diberikan dalam URL.
- ```user = User.query.filter_by(id=id).first()``` yaitu mencari pengguna dalam database berdasarkan ID yang diberikan. Jika ditemukan, objek pengguna disimpan dalam variabel user.
- ```user = User.query.filter_by(id=id).first()``` yaitu mengambil objek pengguna lagi
- ```db.session.delete(user)``` yaitu menghapus objek pengguna dari sesi database.
- ```db.session.commit()``` yaitu melakukan commit perubahan ke dalam database.
- ```return jsonify({"message":"User deleted successfully"})``` yaitu mengembalikan respons JSON yang menyatakan bahwa pengguna telah berhasil dihapus.
- ```return jsonify({"message":"User not found"}), 404``` yaitu jika pengguna tidak ditemukan maka respons JSON dengan pesan "User not found" dan kode status HTTP 404 dikirimkan kembali ke klien.

Script tersebut digunakan untuk membuat endpoint API yang memungkinkan klien untuk menghapus pengguna berdasarkan ID dari database. Saat menerima permintaan DELETE dengan ID pengguna tertentu, fungsi destroy() mencari pengguna dalam database. Jika ditemukan, pengguna dihapus dari database, dan respons JSON yang menyatakan bahwa pengguna telah berhasil dihapus dikirimkan kembali ke klien. Jika pengguna tidak ditemukan, respons JSON dengan pesan "User not found" dan kode status HTTP 404 dikirimkan kembali ke klien.

```python
if _name_ == '_main_':
    app.run(debug=True)
```
Script diatas merupakan bagian akhir dari aplikasi Flask yang berfungsi untuk menjalankan aplikasi ketika skrip ini dijalankan sebagai program utama. 
Berikut penjelasannya :
- ```if __name__ == '__main__'``` merupakan kondisi yang memeriksa apakah script tersebut dijalankan sebagai program utama (main program) dan bukan sebagai modul yang diimpor ke dalam skrip lain.
- ```app.run(debug=True)``` yaitu jika kondisinya benar/true, maka app.run() akan dijalankan dimana ini memulai server pengembangan Flask. *‘debug=True’* digunakan untuk menghidupkan mode debug yang akan memberikan informasi debug tambahan jika terjadi kesalahan.

Script tersebut digunakan untuk memberikan instruksi ke Python tentang bagaimana script ini seharusnya berperilaku ketika dijalankan yang dapat membuat aplikasi web berbasis Flask dapat dijalankan secara mandiri.

## Trigger Log
#### 1. Membuat Tabel *trigger_log_user*
```sql
CREATE TABLE IF NOT EXISTS trigger_log_user (
   log_id serial PRIMARY KEY,
   users_id VARCHAR(255),
   changed_field VARCHAR(255),
   old_value VARCHAR(255),
   new_value VARCHAR(255),
   log_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
Ini adalah perintah SQL untuk membuat tabel *trigger_log_user* di dalam database. Tabel ini akan digunakan untuk mencatat perubahan yang terjadi pada data pengguna (users) dalam tabel lain.

- ```log_id serial``` yang bertindak sebagai primary key.

- ```users_id``` untuk menyimpan ID pengguna yang terkait dengan perubahan.

-	 ```changed_field``` untuk menyimpan nama kolom yang berubah.

- ```old_value``` untuk menyimpan nilai lama dari kolom yang berubah.

- ```new_value``` untuk menyimpan nilai baru dari kolom yang berubah.

- ```log_date``` untuk menyimpan tanggal dan waktu perubahan, dengan default nilai saat ini.

#### 2. Membuat Fungsi *log_users_changes* untuk Digunakan oleh Trigger:
```sql
CREATE OR REPLACE FUNCTION log_users_changes()
RETURNS TRIGGER AS $$
BEGIN
   IF TG_OP = 'UPDATE' THEN
       IF NEW.password IS DISTINCT FROM OLD.password THEN
           INSERT INTO trigger_log_user(users_id, changed_field, old_value, new_value)
           VALUES(NEW.id::text, 'password', OLD.password, NEW.password);
       END IF;

       IF NEW.username IS DISTINCT FROM OLD.username THEN
           INSERT INTO trigger_log_user(users_id, changed_field, old_value, new_value)
           VALUES(NEW.id::text, 'username', OLD.username, NEW.username);
       END IF;

       IF NEW.id IS DISTINCT FROM OLD.id THEN
           INSERT INTO trigger_log_user(users_id, changed_field, old_value, new_value)
           VALUES(OLD.id::text, 'id', OLD.id::text, NEW.id::text);
       END IF;
   ELSIF TG_OP = 'INSERT' THEN
       INSERT INTO trigger_log_user(users_id, changed_field, old_value, new_value)
       VALUES(NEW.id::text, 'username', '', NEW.username),
              (NEW.id::text, 'password', '', NEW.password),
              (NEW.id::text, 'id', '', NEW.id::text);
   ELSIF TG_OP = 'DELETE' THEN
       INSERT INTO trigger_log_user(users_id, changed_field, old_value, new_value)
       VALUES(OLD.id::text, 'username', OLD.username, ''),
              (OLD.id::text, 'password', OLD.password, ''),
              (OLD.id::text, 'id', OLD.id::text, '');
   END IF;

   RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Ini adalah definisi fungsi *log_users_changes()* yang akan digunakan sebagai trigger pada tabel *users*. Fungsi ini akan dieksekusi setiap kali terjadi operasi INSERT, UPDATE, atau DELETE pada tabel *users*. Dalam fungsi ini, setiap perubahan yang terjadi pada kolom password, username, atau id akan dicatat ke dalam tabel *trigger_log_user*, bersama dengan nilai lama dan nilai baru dari kolom yang berubah. Fungsi ini ditulis dalam bahasa PL/pgSQL.


#### 3. Membuat Trigger users_trigger
```sql
CREATE TRIGGER users_trigger
AFTER UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION log_users_changes();
```
Ini adalah perintah SQL untuk membuat trigger *users_trigger* yang akan dipicu setelah terjadi perubahan pada tabel *users*. Setiap kali terjadi perubahan pada tabel *users*, trigger ini akan menjalankan fungsi *log_users_changes()*, yang akan mencatat perubahan ke dalam tabel *trigger_log_user*.

## Test
- Data Awal:

![data awal](https://github.com/daraxnailah/uts-pemrograman-PL.SQL/assets/125743006/567352ea-6898-4151-9858-2e40e5731672)

- Log Trigger for changes username & password:

![Log Trigger for changes username   password](https://github.com/daraxnailah/uts-pemrograman-PL.SQL/assets/125743006/55192716-9d27-4ebd-93fe-37fcad97966d)

- Data Akhir:

![Data Akhir](https://github.com/daraxnailah/uts-pemrograman-PL.SQL/assets/125743006/cf6039bd-95cc-4f54-b64d-50a13a535fd1)

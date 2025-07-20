# n8n Docker Compose Setup
![c08901b3285b983b06900d1d9f9fdb42980685db](https://github.com/user-attachments/assets/c854d31a-e52a-4022-9b65-b7fbf6a7df28)
Dokumentasi ini menjelaskan langkah demi langkah untuk menjalankan [n8n](https://n8n.io/) menggunakan Docker Compose berdasarkan file `docker-compose.yml` yang telah Anda buat.

---

## Daftar Isi

1. [Deskripsi Proyek](#deskripsi-proyek)
2. [Prasyarat](#prasyarat)
3. [Struktur File](#struktur-file)
4. [Penjelasan docker-compose.yml](#penjelasan-docker-composeyml)
5. [Langkah Menjalankan](#langkah-menjalankan)
6. [Akses n8n di Browser](#akses-n8n-di-browser)
7. [Penyimpanan Data](#penyimpanan-data)
8. [Penghentian dan Penghapusan Container](#penghentian-dan-penghapusan-container)
9. [Troubleshooting](#troubleshooting)
10. [Referensi](#referensi)

---

## Deskripsi Proyek

File ini digunakan untuk menjalankan aplikasi n8n (workflow automation tool) secara otomatis menggunakan Docker Compose. Dengan setup ini, Anda dapat menjalankan n8n secara lokal dengan mudah dan data workflow Anda akan tersimpan secara persisten.

---

## Prasyarat

- **Docker** sudah terinstall di komputer Anda  
  [Download Docker Desktop](https://www.docker.com/products/docker-desktop/)
- **Docker Compose** sudah terinstall (biasanya sudah termasuk dalam Docker Desktop)
- File `.env` (opsional, untuk konfigurasi environment n8n)

---

## Struktur File

```
.
├── docker-compose.yml
├── .env                # (opsional, untuk environment variable n8n)
└── n8n_data/           # (otomatis dibuat, untuk menyimpan data n8n)
```

---

## Penjelasan docker-compose.yml

```yaml
version: "3"

services:
  n8n:
    image: n8nio/n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    env_file:
      - .env
    volumes:
      - ./n8n_data:/home/node/.n8n

volumes:
  n8n_data:
    driver: local
```

- **services.n8n.image**: Menggunakan image resmi n8n dari Docker Hub.
- **restart: unless-stopped**: Container akan otomatis restart kecuali dihentikan manual.
- **ports**: Memetakan port 5678 di container ke port 5678 di komputer lokal.
- **env_file**: Menggunakan file `.env` untuk environment variable (opsional).
- **volumes**: Menyimpan data workflow n8n secara persisten di folder `n8n_data` lokal.
- **volumes.n8n_data**: Mendefinisikan volume Docker bernama `n8n_data` dengan driver lokal.

---

## Langkah Menjalankan

1. **Buka Terminal/Command Prompt**  
   Arahkan ke folder tempat file `docker-compose.yml` berada.

   ```sh
   cd "C:\Users\marli\OneDrive\Desktop\New folder"
   ```

2. **Jalankan Docker Compose**

   ```sh
   docker-compose up -d
   ```

   - Opsi `-d` agar berjalan di background (detached mode).

3. **Cek Status Container**

   ```sh
   docker-compose ps
   ```

---

## Akses n8n di Browser

Setelah container berjalan, buka browser dan akses:

```
http://localhost:5678
```

---

## Penyimpanan Data

- Semua data workflow n8n akan disimpan di folder `n8n_data` pada komputer Anda.
- Data ini tetap ada meskipun container dihentikan atau dihapus.

---

## Penghentian dan Penghapusan Container

- **Hentikan container:**
  ```sh
  docker-compose down
  ```
- **Hapus data volume (jika ingin reset data):**
  ```sh
  docker-compose down -v
  ```

---

## Troubleshooting

- **Port 5678 sudah digunakan:**  
  Ubah port di bagian `ports` pada `docker-compose.yml`.
- **Perubahan pada file `.env` tidak terdeteksi:**  
  Restart container dengan `docker-compose down` lalu `docker-compose up -d`.
- **Docker tidak bisa dijalankan:**  
  Pastikan Docker Desktop sudah aktif.
- **Masalah Path/Akses ke Root (/):**
  n8n tidak memiliki route / secara default, Solusi: Akses langsung ke endpoint editor:
 ```sh
  http://localhost:5678/workflow
  ```
atau 
```sh
 http://localhost:5678/login
  ```
- **Cache Browser/Redirect Loop:**
Browser mungkin mencoba redirect ke HTTPS atau menyimpan cache salah. Buka di mode incognito atau gunakan curl untuk test:
```sh
curl -v http://localhost:5678/workflow
  ```
- **Akses URL berikut:**
  `http://localhost:5678/workflow` 
  `Login: http://localhost:5678/login` 
- **Jika masih error cek lognya :**
```sh
docker-compose logs n8n --tail=100
  ```
atau 
```sh
curl -v http://localhost:5678/healthz
  ```
---

## Referensi

- [n8n Documentation](https://docs.n8n.io/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)

---

# Docker Compose - Admin Jualan

Dokumentasi untuk melakukan build Docker image dan menjalankan seluruh service menggunakan `compose.yaml`.

## Prerequisites

Pastikan sudah menginstal:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) atau Docker Engine
- Docker Compose

Pastikan Docker sedang berjalan sebelum menjalankan perintah.

---

## 1. Build Semua Image (Normal)

Build seluruh image yang memiliki konfigurasi `build` di dalam `compose.yaml`.

```bash
docker compose build
```

## 2. Build Semua Image (Tanpa Cache)

Build ulang seluruh image tanpa menggunakan Docker build cache.

```bash
docker compose build --no-cache
```

Gunakan opsi ini ketika ingin memastikan seluruh image dibangun ulang dari awal.

## 3. Build Image Satu per Satu (Normal)

Build image berdasarkan service yang diinginkan.

**Frontend**

```bash
docker compose build frontend
```

**Backend**

```bash
docker compose build backend
```

## 4. Build Image Satu per Satu (Tanpa Cache)

Build image masing-masing service tanpa menggunakan cache.

**Frontend**

```bash
docker compose build --no-cache frontend
```

**Backend**

```bash
docker compose build --no-cache backend
```

## 5. Build dan Menjalankan Container

Setelah image berhasil dibangun, jalankan seluruh container menggunakan:

```bash
docker compose up -d
```

Keterangan:

- `up`: Membuat dan menjalankan container.
- `-d`: Menjalankan container di background (detached mode).

Jika ingin sekaligus melakukan build image sebelum menjalankan container:

```bash
docker compose up -d --build
```

## 6. Pull Latest Images

Mengunduh image terbaru dari Docker Registry sesuai tag yang didefinisikan di `compose.yaml`.

```bash
docker compose pull
```

Perintah ini digunakan untuk memperbarui prebuilt images yang digunakan oleh services berikut:

- Nginx (`nginx:alpine`)
- PostgreSQL (`postgres:17`)
- Cloudflared (`cloudflare/cloudflared:latest`)

**Setelah pull selesai, jalankan container kembali:**

```bash
docker compose up -d
```

Docker akan menggunakan image yang sudah diperbarui dan membuat ulang container jika diperlukan.

**Pull image untuk service tertentu:**

```bash
docker compose pull nginx
docker compose pull db
docker compose pull cloudflared
```

**Notes:**

- `docker compose pull` hanya mengunduh image, tidak langsung menjalankan atau mengganti container yang sedang berjalan.
- Image yang digunakan mengikuti tag pada `compose.yaml`.
- Tag `latest` tidak otomatis diperbarui tanpa melakukan pull.
- Untuk update PostgreSQL, pastikan melakukan backup database terlebih dahulu, terutama jika melakukan upgrade major version.

## 7. Container Management

**Check container status**

```bash
docker compose ps
```

**View all logs**

```bash
docker compose logs -f
```

**View specific service logs**

```bash
docker compose logs -f backend
```

**Stop all containers**

```bash
docker compose stop
```

**Stop and remove containers**

```bash
docker compose down
```

**Restart all services**

```bash
docker compose restart
```

## 8. Service Information

| Service     | Description              | Port           |
| ----------- | ------------------------ | -------------- |
| frontend    | React + Vite application | Internal: 80   |
| backend     | NestJS API               | Internal: 8080 |
| nginx       | Reverse proxy            | 3000:80        |
| db          | PostgreSQL 17            | 5434:5432      |
| cloudflared | Cloudflare Quick Tunnel  | -              |

## 9. Important Notes

- Frontend dan Backend menggunakan Dockerfile masing-masing.
- Nginx, PostgreSQL, dan Cloudflared menggunakan prebuilt images.
- `docker compose build` hanya membangun custom images.
- `docker compose pull` mengunduh image dari registry.
- `--no-cache` hanya menonaktifkan build cache, bukan menghapus images, containers, atau volumes.
- Database menggunakan named volume `postgres_data`, sehingga data tetap tersimpan meskipun container dihentikan atau dihapus menggunakan `docker compose down`.
- Jangan menggunakan `docker compose down -v` kecuali memang ingin menghapus volume database beserta datanya.
- Pastikan `DB_PASSWORD` tersedia di file `.env` root project sebelum menjalankan Compose.

## Quick Reference

| Kebutuhan                     | Command                                    |
| ----------------------------- | ------------------------------------------ |
| Build semua image             | `docker compose build`                     |
| Build semua tanpa cache       | `docker compose build --no-cache`          |
| Build frontend                | `docker compose build frontend`            |
| Build backend                 | `docker compose build backend`             |
| Build frontend tanpa cache    | `docker compose build --no-cache frontend` |
| Build backend tanpa cache     | `docker compose build --no-cache backend`  |
| Build nginx, postgreSql, etc  | `docker compose pull`                      |
| Menjalankan container         | `docker compose up -d`                     |
| Build + menjalankan container | `docker compose up -d --build`             |
| Melihat container             | `docker compose ps`                        |
| Melihat logs                  | `docker compose logs -f`                   |
| Menghentikan container        | `docker compose down`                      |

🚀 Gambaran Umum
Network Interface Manager adalah script Python yang dirancang untuk mengelola dan mengamankan interface jaringan di Linux. Script ini secara otomatis mengidentifikasi dan menonaktifkan interface jaringan yang tidak digunakan untuk meningkatkan keamanan sistem.

🔍 Fitur Utama
🛡️ Manajemen Interface Cerdas
Deteksi Otomatis - Mengidentifikasi semua interface jaringan yang tersedia

Analisis Status - Memeriksa status UP/DOWN setiap interface

Validasi IP Address - Memverifikasi interface yang memiliki alamat IP

Kriteria Penonaktifan - Hanya menonaktifkan interface yang memenuhi kriteria keamanan

🔒 Kriteria Penonaktifan Aman
Interface akan dinonaktifkan jika memenuhi kedua kondisi berikut:

✅ Status DOWN - Interface tidak aktif

✅ Tidak memiliki IP Address - Tidak ada konfigurasi IP

❌ Bukan loopback - Interface lo tidak akan pernah dinonaktifkan

⚙️ Fitur Keamanan
Konfirmasi Manual - Meminta persetujuan user sebelum menonaktifkan

Error Handling - Penanganan error yang robust

Logging Detail - Laporan lengkap setiap aksi yang dilakukan

🏗️ Cara Kerja
Alur Eksekusi:
Scan Interface - Mendeteksi semua interface jaringan (kecuali loopback)

Analisis Status - Memeriksa status dan konfigurasi IP setiap interface

Evaluasi Kriteria - Menentukan interface yang memenuhi syarat penonaktifan

Konfirmasi User - Meminta persetujuan sebelum menonaktifkan

Eksekusi - Menonaktifkan interface yang disetujui

Laporan - Menampilkan ringkasan aksi yang dilakukan

📦 Instalasi & Persyaratan
Persyaratan Sistem
OS: Linux (DevAsc, Ubuntu, Debian, dll.)

Python: Version 3.6 atau lebih tinggi

Permissions: Akses sudo untuk menonaktifkan interface

Tools: iproute2 (perintah ip)

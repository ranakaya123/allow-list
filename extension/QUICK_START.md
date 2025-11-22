# 🚀 Keepnet Extension - Hızlı Başlangıç

## ⚡ 3 Adımda Başlat

### 1️⃣ Extension'ı Yükle

```bash
1. Chrome'da chrome://extensions aç
2. "Developer mode" açık olsun (sağ üst)
3. "Load unpacked" tıkla
4. Bu klasörü seç: /Users/rozerinozturk/Desktop/white v/extension
5. ✅ Extension yüklendi!
```

### 2️⃣ Security Center'a Git

```bash
1. https://security.microsoft.com/homepage aç
2. Extension icon'una (🛡️) tıkla
3. SOL-ALT köşede panel açılacak
```

### 3️⃣ Asistan Çalışıyor!

Panel açıldı ve **Adım 1: Security Center Ana Sayfası** gösterildi ✅

---

## 🐛 Sorun Giderme

### Panel Açılmıyor?

**Konsolu aç** (F12 veya Sağ tık → İncele → Console)

Şu mesajları ara:

```javascript
[Keepnet v3.1] Content script loaded
[Keepnet] Initializing assistant...
[Keepnet] Assistant ready!
```

**Eğer hiç mesaj yoksa:**
```bash
1. Extension sayfasına git (chrome://extensions)
2. Keepnet Extension'ı bul
3. "Reload" (🔄) butonu tıkla
4. Sayfayı yenile (F5)
5. Extension icon'una tekrar tıkla
```

**Eğer hata mesajı varsa:**
```bash
Hatayı kopyala ve bana gönder!
```

### Panel Açılıp Kapanıyor?

**Console'da hata ara:**

```javascript
[Keepnet] executeStep error: ...
// veya
[Keepnet] Init error: ...
```

**Çözüm:**
```bash
1. Chrome DevTools Console'da hatayı bul
2. Hatanın tam metnini kopyala
3. Bana gönder
```

### Butonlar Çalışmıyor?

**"Devam Et" butonu çalışmıyor mu?**

Console'da şunu ara:
```javascript
[Keepnet] Next button clicked
```

**Eğer bu mesaj yoksa:**
```bash
1. Paneli kapatıp tekrar aç
2. Veya sayfayı yenile
```

---

## 📋 Test Adımları

### Adım 1: Panel Görünümü Test

**Beklenen:**
- ✅ Sol-alt köşede 340x520px panel
- ✅ Header: "Keepnet Allow List" + "Adım 1 / 12"
- ✅ Body: "Security Center Ana Sayfası" açıklaması
- ✅ Footer: "← Geri" ve "Devam Et →" butonları

**Test:**
```bash
1. Paneli sürükle → konum kaydediliyor mu?
2. Sayfayı yenile → panel aynı yerde mi?
3. X butonuna tıkla → panel kapanıyor mu?
4. Extension icon'una tıkla → panel tekrar açılıyor mu?
```

### Adım 2: "Devam Et" Butonu Test

**Console açık olsun!**

```bash
1. "Devam Et" butonuna tıkla
2. Console'da "Next button clicked" görünmeli
3. Adım 2'ye geçmeli: "Email and collaboration"
4. Sayfa yüklenince element highlight olmalı
```

### Adım 3: Auto-Click Test

**Adım 2'de:**
```bash
1. 5 saniye bekle
2. Console'da "Auto-click in 4s..." countdown görmeli
3. 5 saniye sonunda otomatik tıklama yapmalı
4. "Email and collaboration" menüsü açılmalı
```

---

## 🔍 Console Mesajları (Normal Akış)

```javascript
[Keepnet v3.1] Content script loaded on https://security.microsoft.com/homepage
[Keepnet v3.1] Background service worker started
[Keepnet] Extension clicked on tab: 123456
[Keepnet] Starting assistant on tab: 123456
[Keepnet] Initializing assistant...
[Keepnet] Button handlers attached
[Keepnet] Executing step 1: Security Center Ana Sayfası
[Keepnet] Assistant ready!

// Devam Et tıklanınca:
[Keepnet] Next button clicked
[Keepnet] Executing step 2: Email and Collaboration
[Keepnet] Auto-click in 5s...
[Keepnet] Auto-click in 4s...
[Keepnet] Auto-click in 3s...
[Keepnet] Auto-click in 2s...
[Keepnet] Auto-click in 1s...
[Keepnet] Auto-clicking element: <button>
[Keepnet] Element clicked for step 2
[Keepnet] Capturing screenshot for step2_emailcollab...
[Keepnet] Screenshot captured: step2_emailcollab.png (245KB)
```

---

## 🎯 Beklenen Davranış

### Her Adımda:

1. ✅ Panel içeriği güncellenir
2. ✅ Progress bar ilerler (0% → 8% → 17% ...)
3. ✅ Element yeşil border ile highlight edilir
4. ✅ Tooltip gösterilir: "📧 Email and collaboration'a tıklayın"
5. ✅ 5 saniye içinde otomatik tıklama başlar
6. ✅ Manuel tıklama auto-click'i iptal eder
7. ✅ Validation yapılır
8. ✅ Screenshot otomatik alınır
9. ✅ Sonraki adıma geçilir

### Kritik Adım 8 (IP Ekleme):

1. ✅ Panel highlight eder IP alanını
2. ✅ Tooltip: "🔢 IP'leri ekleyin: 149.72.161.59, ..."
3. ✅ Kullanıcı IP girdikçe real-time validation
4. ✅ Eksik IP varsa: "❌ IP girmeyi unuttunuz: 149.72.42.201"
5. ✅ Tüm IP'ler girilince: "✅ Tüm IP'ler eklendi! (3/3)"
6. ✅ 10 saniye hiç giriş yoksa: "IP'leri otomatik doldurmak ister misiniz?"

### Adım 12 (Özet):

```
📊 Özet Rapor

✅ Security Center Ana Sayfası
   Screenshot: step1_home.png

✅ Email and Collaboration
   Screenshot: step2_emailcollab.png

❌ Phishing Simulation Sekmesi
   [Git ve Düzelt]

⏳ IP Adresleri Ekleme
   [Git ve Düzelt]

💾 Tüm screenshot'lar chrome.storage'da kaydedildi
```

---

## 🆘 Hata Mesajları

| Mesaj | Sebep | Çözüm |
|-------|-------|-------|
| `❌ Element bulunamadı` | Selector yanlış veya sayfa yüklenmedi | Manuel devam et |
| `⚠️ Element bulunamadı: ...` | Element görünür değil | Sayfayı scroll et, tekrar dene |
| `❌ Hata: ...` | JavaScript exception | Console'u kontrol et |
| `❌ IP girmeyi unuttunuz: ...` | Bazı IP'ler eksik | Eksik IP'leri ekle |
| `Keepnet Asistanı başlatılamadı` | Init hatası | Sayfayı yenile |

---

## 💡 İpuçları

### 1. DevTools Her Zaman Açık

```bash
F12 tuşuna bas
Console sekmesini seç
Hataları/mesajları takip et
```

### 2. Panel Kayboldu mu?

```bash
Extension icon'una tekrar tıkla
veya
Sayfayı yenile (F5)
```

### 3. Auto-Click İptal Etme

```bash
Element'e manuel tıkla
→ Auto-click countdown durur
```

### 4. Adım Atlama

```bash
"Devam Et" butonuna tıkla
→ Mevcut adımı geç (validation olmadan)
```

### 5. Screenshot'ları Görme

```bash
Console'da: chrome.storage.local.get(['keepnet_screenshots_v3'], console.log)
→ Tüm screenshot'lar (base64) görünür
```

---

## 📞 Destek

**Sorun mu yaşıyorsun?**

1. ✅ Console screenshot'ı al (F12 → Console)
2. ✅ Hata mesajını kopyala
3. ✅ Hangi adımda olduğunu söyle
4. ✅ Bana gönder!

**Örnek Rapor:**

```
Sorun: Panel açılıp kapanıyor
Adım: 1 (Security Center Ana Sayfası)
Console:
[Keepnet] Init error: Cannot read property 'updateProgress' of null
```

---

## 🎉 Başarılı Kurulum Testi

**Tüm bunlar çalışıyorsa kurulum başarılı:**

- ✅ Panel sol-alt köşede açılıyor
- ✅ "Devam Et" butonu çalışıyor
- ✅ Console'da mesajlar görünüyor
- ✅ Progress bar ilerliyor
- ✅ Element highlighting çalışıyor

**Şimdi gerçek testi yap:**

```bash
1. https://security.microsoft.com/homepage sayfasına git
2. Extension'ı başlat
3. Adım adım ilerle
4. Her adımda console'u kontrol et
5. Screenshot'ların alındığını doğrula
```

---

🛡️ **Keepnet Allow List Asistanı v3.1**

*Sol-alt köşede, tek panel, otomatik tıklama, gerçek zamanlı validation!*


 
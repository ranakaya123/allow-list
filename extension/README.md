# Keepnet Office 365 Allow List Asistanı v3.0

**Müşterilere Office 365'te Keepnet phishing simülasyonu allow list yapılandırmasını adım adım öğreten, gerçek zamanlı doğrulama yapan ve kanıt toplayan interaktif Chrome Extension.**

---

## 🎯 Özellikler

### ✨ v3.0 Yenilikleri

1. **Tek Panel Tasarımı**
   - Sadece küçük, zarif floating panel
   - 3 boyut seçeneği: Küçük (320px), Orta (400px), Büyük (480px)
   - Otomatik minimize: 45 saniye inaktiflikten sonra FAB (Floating Action Button)
   - Sürükle-bırak ile konumlandırma
   - Konum hafızası

2. **Akıllı Navigasyon Sistemi**
   - Her adım için otomatik URL yönlendirmesi
   - Element bulma ve highlighting
   - **Öğrenme sistemi**: Kullanıcı bir elementi tıklarken CSS selector'ı öğrenir
   - Gelecek kullanımlarda otomatik element bulma
   - Scroll ve focus yönetimi

3. **Gerçek Zamanlı Doğrulama**
   - **IP Validation**: Eksik IP'leri anında gösterir
   - **Domain Validation**: Wildcard format kontrolü
   - **Checkbox Validation**: Safe Links, Office 365 Apps ayarları
   - **Header Validation**: Mail flow kurallarında header kontrolü
   - **SCL Validation**: Spam confidence level kontrolü
   - MutationObserver ile input field'ları dinler
   - Her 2 saniyede periyodik kontrol

4. **Ekran Görüntüsü + Kanıt Sistemi**
   - Her adımda otomatik screenshot
   - Validation sonuçlarıyla birlikte saklanır
   - Base64 format, chrome.storage.local'de
   - Özet ekranında thumbnail görüntüleme

5. **Detaylı Özet Ekranı**
   - Tüm adımların durumu: ✅ Tamamlandı / ❌ Eksik / ⏳ Bekliyor
   - Her adım için detaylı feedback
   - Screenshot thumbnail'ları (tıklayınca büyür)
   - "Git ve Düzelt" butonları ile doğrudan yönlendirme
   - Flow'lara göre gruplandırılmış görünüm

6. **İlerleme Takibi**
   - Panel header'da progress bar
   - "Adım X / Y" göstergesi
   - Dinamik ilerleme yüzdesi

---

## 📂 Dosya Yapısı

```
extension/
├── manifest.json          # Extension manifest (v3)
├── background.js          # Service worker (navigasyon, screenshot)
├── content.js             # Ana script (6 major class içerir)
├── content.css            # Minimal CSS (animasyonlar)
├── config.json            # 43 adımlık konfigürasyon (PDF referansları)
├── steps.json             # Alternatif config (legacy)
├── icons/                 # Extension iconları
│   ├── icon16.jpg
│   ├── icon48.jpg
│   └── icon128.jpg
└── README.md              # Bu dosya
```

---

## 🔧 Mimari

### content.js Ana Sınıflar

1. **FloatingPanel** (Part 2)
   - Tek panel yönetimi
   - Auto-minimize logic
   - FAB (Floating Action Button)
   - Drag & drop konumlandırma

2. **RealTimeValidator** (Part 3)
   - MutationObserver ile input dinleme
   - IP, domain, wildcard validation
   - Checkbox, header, SCL kontrolü
   - Callback sistem ile anlık feedback

3. **NavigationEngine** (Part 4)
   - Akıllı sayfa geçişi
   - Element bulma (selector matching)
   - Öğrenme sistemi (learned selectors)
   - Highlight & tooltip

4. **ScreenshotManager** (Part 5)
   - chrome.tabs.captureVisibleTab
   - Storage yönetimi
   - Validation sonuçlarıyla eşleştirme

5. **SummaryView** (Part 6)
   - Detaylı özet rendering
   - Step status (completed/missing/pending)
   - Screenshot thumbnail display
   - "Go & Fix" aksiyon butonları

6. **KeepnetAssistant** (Orchestrator)
   - Tüm sınıfları koordine eder
   - Step transition
   - Message handling
   - Global state management

---

## 🚀 Kullanım

### Kurulum

1. Chrome'da `chrome://extensions` aç
2. "Developer mode" aktif et
3. "Load unpacked" → `extension/` klasörünü seç
4. Extension yüklendi ✅

### Başlatma

1. [Microsoft Security Center](https://security.microsoft.com) veya [Exchange Admin](https://admin.exchange.microsoft.com) sayfasına git
2. Extension icon'una tıkla 🛡️
3. Floating panel açılır → Adım 1 başlar

### İş Akışı

1. **Panel** sağ-alt köşede açılır
2. **Adım açıklaması** gösterilir
3. **Otomatik navigasyon**: Doğru URL'ye yönlendirilirsin
4. **Element highlighting**: İlgili buton/input yeşil border ile vurgulanır
5. **Gerçek zamanlı validation**: IP/domain girerken anlık feedback
6. **"Devam Et"** butonuna tıkla → Screenshot alınır + sonraki adıma geçilir
7. **Özet** butonuna tıkla → Tüm adımların durumunu gör

---

## 📖 Config Yapısı (config.json)

```json
{
  "meta": {
    "critical": {
      "ips": ["149.72.161.59", "149.72.42.201", "149.72.154.87"],
      "domains": ["*.keepnetlabs.com", "*.simulation.keepnetlabs.com"]
    }
  },
  "flows": [
    {
      "id": "advanced_delivery",
      "site": "security.microsoft.com",
      "name": "Third-party Phishing Simulations",
      "steps": [
        {
          "id": 6,
          "title": "Sending IP Ekleme",
          "description": "Keepnet IP adreslerini ekleyin",
          "navigate": { "url": "https://security.microsoft.com/advanceddelivery" },
          "validate": "sending_ip",
          "required": ["meta.critical.ips"],
          "selectors": ["input[aria-label*='Sending IP']"],
          "realTimeValidation": true,
          "expectedValues": ["149.72.161.59", "149.72.42.201", "149.72.154.87"],
          "pdfReference": "Sayfa 5, Step 6"
        }
      ]
    }
  ],
  "validations": {
    "sending_ip": {
      "type": "ip_list",
      "required": ["149.72.161.59", "149.72.42.201", "149.72.154.87"]
    }
  }
}
```

### Step Config Özellikleri

- `id`: Adım numarası
- `title`: Adım başlığı
- `description`: Açıklama
- `navigate.url`: Otomatik yönlendirme URL'i
- `selectors[]`: Element bulma CSS selector'ları
- `validate`: Validation tipi (sending_ip, domains_mail_from, vb.)
- `realTimeValidation`: true ise anlık validation başlar
- `expectedValues[]`: Beklenen değerler (IP, domain listesi)
- `pdfReference`: PDF kaynak referansı

---

## 🎨 UI/UX Detayları

### Panel Boyutları

```javascript
const PANEL_SIZES = {
  small: { width: 320, height: 480 },
  medium: { width: 400, height: 600 },
  large: { width: 480, height: 720 }
}
```

### Renkler

- **Primary Gradient**: `#667eea → #764ba2` (Header, buttons)
- **Success**: `#22c55e` (Validation OK, highlight)
- **Error**: `#dc2626` (Validation fail)
- **Warning**: `#f59e0b`
- **Background**: `#f9fafb`

### Animasyonlar

- **Slide In**: Panel açılış (cubic-bezier bounce)
- **Pulse**: Element highlighting (2s infinite)
- **Fade In**: Screenshot modal
- **Highlight**: Yeşil glow effect (3x iteration)

---

## 🔍 Validation Tipleri

| Tip | Açıklama | Kontrol Edilen |
|-----|----------|----------------|
| `sending_ip` | IP listesi | 3 Keepnet IP'si var mı? |
| `domains_mail_from` | Domain | Keepnet domain'i var mı? |
| `simulation_urls_wildcard` | Wildcard domain | `*.domain.com/*` formatı |
| `track_clicks_off` | Checkbox | "Track user clicks" OFF mu? |
| `scl_minus_one` | SCL değeri | SCL = -1 ayarlanmış mı? |
| `bypass_clutter_header` | Mail header | X-MS-Exchange-Organization-BypassClutter = true |
| `skip_safe_links_header` | Mail header | X-MS-Exchange-Organization-SkipSafeLinksProcessing = 1 |

---

## 💾 Storage Yapısı

```javascript
STORAGE_KEYS = {
  PANEL_STATE: 'keepnet_panel_state_v3',      // Panel konumu, minimize durumu
  STEP_RESULTS: 'keepnet_step_results_v3',    // Her adımın validation sonucu
  LEARNED_SELECTORS: 'keepnet_learned_selectors_v3', // Öğrenilen CSS selector'lar
  SCREENSHOTS: 'keepnet_screenshots_v3',      // Screenshot'lar (base64)
  PROGRESS: 'keepnet_progress_v3',            // Mevcut adım
  SETTINGS: 'keepnet_settings_v3'             // Panel boyutu, auto-minimize ayarı
}
```

---

## 🧪 Test Senaryosu

1. **Extension'ı başlat**
   - Icon'a tıkla
   - Panel açılır, Adım 1 görünür

2. **Adım 6'ya git** (Sending IP)
   - Otomatik olarak Advanced Delivery sayfasına yönlendirilir
   - IP input field'ı yeşil border ile highlight edilir
   - Tooltip gösterilir

3. **IP girmeye başla**
   - İlk IP'yi gir: `149.72.161.59`
   - Anlık feedback: "⚠️ Eksik IP'ler: 149.72.42.201, 149.72.154.87"
   - Kalan IP'leri ekle
   - Feedback: "✅ Tüm IP'ler eklendi"

4. **"Devam Et" tıkla**
   - Screenshot otomatik alınır
   - Adım 7'ye geçilir

5. **Özet'e tıkla**
   - Tüm adımların listesi gösterilir
   - Adım 6: ✅ Tamamlandı (screenshot thumbnail)
   - Adım 7: ⏳ Bekliyor
   - "Git ve Düzelt" butonu → o adıma yönlendirir

---

## 📝 PDF Referansları

Config dosyasındaki her adım, Keepnet'in "white.pdf" dökümanındaki ilgili sayfa ve adıma referans verir:

- **Sayfa 4-5**: Advanced Delivery
- **Sayfa 5-6**: Connection Filter
- **Sayfa 6-7**: Safe Links Policy
- **Sayfa 7-8**: Mail Flow Rules - Bypass Spam
- **Sayfa 8-9**: Skip Safe Links Processing
- **Sayfa 10-11**: Skip Safe Attachments Processing
- **Sayfa 11-12**: Troubleshooting - Message Trace

---

## 🛠️ Geliştirme Notları

### Öğrenme Sistemi

```javascript
// Kullanıcı bir elemente tıkladığında:
const selector = Utils.buildUniqueSelector(element)
navigationEngine.learnSelector(stepId, selector)

// Sonraki kullanımda:
const learnedSelector = this.learnedSelectors[stepId]
const element = document.querySelector(learnedSelector)
if (element) highlightElement(element)
```

### Screenshot Akışı

```javascript
// 1. Content script → Background
chrome.runtime.sendMessage({ action: 'captureScreenshot', step: 6 })

// 2. Background → chrome.tabs.captureVisibleTab
const dataUrl = await chrome.tabs.captureVisibleTab(null, { format: 'png' })

// 3. Background → Content (mesaj geri gönder)
chrome.tabs.sendMessage(tabId, { action: 'screenshotCaptured', dataUrl })

// 4. Content → Storage'a kaydet
screenshotManager.save(stepId, dataUrl, validationResult)
```

---

## 🚧 Gelecek Geliştirmeler

- [ ] Çoklu dil desteği (EN, TR dinamik değişimi)
- [ ] Dark mode
- [ ] Export summary as PDF/HTML rapor
- [ ] Keyboard shortcuts (N: Next, P: Prev, S: Summary)
- [ ] Candidate element selection UI (birden fazla element bulunduğunda)
- [ ] Undo/Redo navigation
- [ ] Session replay (adımları tekrar göster)

---

## 📄 Lisans

© 2025 Keepnet Labs. Tüm hakları saklıdır.

---

## 👨‍💻 Geliştirici

**Versiyon**: 3.0.0  
**Son Güncelleme**: 22 Ekim 2025  
**Gereksinimler**: Chrome 88+, Manifest v3  

**Not**: Bu extension Microsoft 365 Security Center ve Exchange Admin Center'da çalışır. Keepnet IP'leri ve domain'leri config.json dosyasında tanımlıdır.

 
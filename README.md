# Şile Hanımeli Antik Cafe — Web Sitesi

Statik HTML/CSS web sitesi. Build adımı yok — git'e push, sunucuda pull, biter.

## Yapı

```
hanimeliantikcafe/
├── index.html          # Anasayfa (hero, hakkımızda, hizmetler, galeri, iletişim)
├── menu.html           # Tüm kategorilerle birlikte menü sayfası
├── qr.html             # Yazdırılabilir karekod sayfası (masa kartı için)
├── robots.txt          # SEO
├── sitemap.xml         # SEO
├── .gitignore
├── README.md
└── assets/
    ├── css/style.css   # Tüm stiller (paylaşımlı)
    ├── img/            # Görseller + favicon.svg
    └── js/
```

## Yerel Önizleme

Yerel sunucu olmadan da çalışır ama `qr.html`'in `localStorage` ve `file://` davranışı için basit bir sunucu önerilir:

```bash
# Python ile
python -m http.server 8000

# Veya Node ile
npx serve .
```

Ardından `http://localhost:8000` adresini açın.

## Git Reposuna Bağlama

```bash
cd "c:/Users/mert/Desktop/hanimeliantikcafe"
git init
git add .
git commit -m "İlk sürüm: anasayfa, menü ve karekod"
git branch -M main
git remote add origin <REPO_URL>
git push -u origin main
```

## Sunucu Deployu (git pull yöntemi)

Sunucuda (örn. Nginx/Apache web kök klasörü `/var/www/hanimeli`):

**İlk kurulum:**
```bash
cd /var/www
git clone <REPO_URL> hanimeli
```

**Güncelleme:**
```bash
cd /var/www/hanimeli
git pull origin main
```

İstersen otomatikleştirmek için bir **post-receive hook** veya **GitHub Actions / Webhook** kurabiliriz.

### Nginx örnek konfig

```nginx
server {
    listen 80;
    server_name hanimeliantikcafe.com www.hanimeliantikcafe.com;
    root /var/www/hanimeli;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # Cache static assets
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|webp)$ {
        expires 30d;
        add_header Cache-Control "public, no-transform";
    }
}
```

SSL için Let's Encrypt: `sudo certbot --nginx -d hanimeliantikcafe.com -d www.hanimeliantikcafe.com`

## SEO Notları (sonradan iyileştirilecek)

Şu an hazır olanlar:
- ✅ Meta title, description, keywords
- ✅ Open Graph + Twitter Card
- ✅ Canonical URL
- ✅ Schema.org `Restaurant` ve `Menu` yapılandırılmış verisi
- ✅ Sitemap.xml + robots.txt
- ✅ Mobile-friendly viewport
- ✅ Anlamsal HTML (header/main/section/footer)
- ✅ Alt metinli görseller
- ✅ `lang="tr"` ve `tr_TR` locale

Eklenebilecekler (domain hazır olunca):
- 🔜 Google Search Console doğrulaması
- 🔜 Google Business Profile bağlantısı (Google Maps konum koordinatları schema'da güncellenmeli)
- 🔜 Gerçek koordinatları (latitude/longitude) `index.html` içindeki Schema.org'a yaz
- 🔜 OG görselini optimize et (1200x630 önerilir)
- 🔜 `assets/img` görsellerini `webp` formatına dönüştür (boyut/hız için)

## Karekod Kullanımı

`qr.html` sayfasını açınca menü URL'sine karekod otomatik üretilir.

- **Yazdır:** A4 sayfaya basıp masalara koyabilirsiniz.
- **PNG İndir:** Karekodu görsel olarak indirip baskı malzemelerinde kullanabilirsiniz.
- **URL Değiştir:** Domain henüz yayında değilken farklı adresler ile deneme yapabilirsiniz (tarayıcıya kaydedilir).

Yayına alınınca, `qr.html` üzerindeki varsayılan adres otomatik olarak `https://hanimeliantikcafe.com/menu.html` olur.

## Domain & Hosting Önerileri

- Domain: `hanimeliantikcafe.com` veya `silehanimeli.com` (Instagram'la uyumlu)
- Türkiye için hosting: Natro, Hostinger, Turhost (statik için yeterli)
- Daha pratik / ücretsiz: **Cloudflare Pages**, **Netlify** veya **Vercel** (git'ten otomatik deploy)
  - Bu durumda sunucu yönetimine gerek kalmaz; git'e push → otomatik canlı.

## Düzenleme İpuçları

- **Fiyat değişikliği:** `menu.html` içindeki `<div class="price">XXX ₺</div>` satırlarını düzenle.
- **Yeni ürün ekle:** Aynı dosyada bir `.menu-item` bloğunu kopyala, içeriğini değiştir.
- **Renk değişikliği:** `assets/css/style.css` en üstteki `:root` bloğundaki CSS değişkenleri.
- **İletişim numarası:** Hem `index.html` hem `menu.html` içinde — `tel:+90...` linklerini ve görünür numaraları güncelle.

## İletişim

- Adres: Çavuş, Ayazma Cd. No:18-1, 34980 Şile/İstanbul
- Telefon: 0539 663 83 62
- Catering: 0544 450 74 24 — 0534 499 21 73
- Instagram: [@silehanimeli](https://instagram.com/silehanimeli)

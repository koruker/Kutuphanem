# Ciltli

Kişisel kitap tarama ve okuma uygulaması. Sayfaları telefon kamerasıyla tarar, kırpıp
tarama filtresi uygular, kitap kapağı çevirme animasyonuyla okur. Her şey telefonun
kendi tarayıcı belleğinde (IndexedDB) saklanır — sunucuya hiçbir fotoğraf gitmez.

## GitHub Pages ile yayınlama

1. Bu klasördeki dosyaları (`index.html`, `manifest.json`, `sw.js`, `icon.svg`) bir GitHub
   deposuna yükle (repo kökü ya da `/docs` klasörü — ikisi de olur).
2. Depo ayarlarında **Settings → Pages** kısmına git, kaynak olarak ilgili branch/klasörü seç.
3. Birkaç dakika sonra `https://kullanici-adin.github.io/repo-adi/` adresinden erişilebilir olur.
4. Telefonda Safari veya Chrome'da bu adresi aç, paylaş menüsünden **"Ana Ekrana Ekle"**
   seçeneğini kullan — uygulama simgeyle, tam ekran ve çevrimdışı çalışır hale gelir.

## Notlar

- Kamera izni istenir; reddedilirse veya desteklenmezse otomatik olarak galeriden
  fotoğraf seçmeye geçer.
- Kırpma ekranında köşeleri sürükleyerek dikdörtgen alanı ayarlayabilir, "Orijinal /
  Siyah-Beyaz Tarama / Netleştir" filtrelerinden birini seçebilirsin.
- Veriler tamamen cihaz üzerinde tutulur; tarayıcı verilerini temizlersen kütüphane de
  silinir. Farklı bir cihazdan aynı kütüphaneye erişilemez (senkronizasyon yok).
- HTTPS gerektiren kamera erişimi için GitHub Pages zaten uygundur (otomatik HTTPS verir).

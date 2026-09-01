# Kütüphanem

Kişisel kitap tarama ve okuma uygulaması. Sayfaları telefon kamerasıyla tarar, kırpıp
döndürüp aynalayarak düzenler, tarama filtresi uygular, kitap kapağı çevirme
animasyonuyla okur. Her şey telefonun kendi tarayıcı belleğinde (IndexedDB) saklanır —
sunucuya hiçbir fotoğraf gitmez. iPhone'da, sadece dikey konumda kullanılacak şekilde
tasarlandı.

## GitHub Pages ile yayınlama

1. Bu klasördeki dosyaları (`index.html`, `manifest.json`, `sw.js`, `icon.svg`,
   `icon-180.png`, `icon-192.png`, `icon-512.png`) bir GitHub deposuna yükle
   (repo kökü ya da `/docs` klasörü — ikisi de olur).
2. Repo **public** olmalı — private repoda Pages özelliği ücretsiz hesaplarda açılmıyor.
   Zaten private oluşturduysan **Settings → General → Danger Zone → Change visibility**
   kısmından public'e çevirebilirsin (kod görünür olur, ama taradığın fotoğraflar
   koda değil, senin telefonundaki tarayıcı belleğine kaydedildiği için hiçbir zaman
   GitHub'a yüklenmez).
3. **Settings → Pages** kısmına git, kaynak olarak ilgili branch/klasörü seç.
4. Birkaç dakika sonra `https://kullanici-adin.github.io/repo-adi/` adresinden erişilebilir olur.
5. iPhone'da Safari'de bu adresi aç, paylaş menüsünden **"Ana Ekrana Ekle"**
   seçeneğini kullan — uygulama kendi simgesiyle, tam ekran ve çevrimdışı çalışır hale gelir.

## Kırpma ve düzenleme

- **Yakınlaştır/uzaklaştır**: iki parmakla pinch yap.
- **Kaydır**: tek parmakla sürükle.
- **Kırpma alanı**: dört köşedeki tutamaçları sürükleyerek ayarla.
- **Döndür**: üstteki döngü ikonuna her basışta 90° döner.
- **Aynala**: yatay olarak ters çevirir (ok simgesi).
- **Sıfırla**: tüm ayarları (döndürme, aynalama, yakınlaştırma) başa alır.
- Filtreler: Orijinal / Siyah-Beyaz Tarama / Netleştir.

## Dikey kilit

iOS Safari'de bir web sayfası ekran yönünü zorla kilitleyemiyor (bu Apple'ın kısıtlaması,
uygulamanın değil). Bunun yerine cihaz yatay çevrildiğinde ekranı kaplayan bir uyarı
("Lütfen telefonunu dik tut") gösteriyoruz, böylece uygulama pratikte sadece dikey
kullanılabiliyor. `manifest.json` içinde de `"orientation": "portrait"` tanımlı; bu,
Ana Ekrana Ekle ile açılan bazı tarayıcılarda ek bir katkı sağlar.

## Notlar

- Kamera izni istenir; reddedilirse veya desteklenmezse otomatik olarak galeriden
  fotoğraf seçmeye geçer.
- Veriler tamamen cihaz üzerinde tutulur; tarayıcı verilerini temizlersen kütüphane de
  silinir. Farklı bir cihazdan aynı kütüphaneye erişilemez (senkronizasyon yok).
- HTTPS gerektiren kamera erişimi için GitHub Pages zaten uygundur (otomatik HTTPS verir).
- Kütüphane mantığı sınırsız kitap içindir: her kitabı "+" ile ayrı ayrı oluşturup
  taruyabilir, rafta hepsini bir arada tutabilirsin.


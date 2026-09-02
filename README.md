# Kütüphanem

Kişisel kitap tarama ve okuma uygulaması. Sayfaları telefon kamerasıyla tarar, kırpıp
döndürüp aynalayarak düzenler, isteğe bağlı olarak yapay zekayla otomatik doğrultur,
tarama filtresi uygular, kitap kapağı çevirme animasyonuyla okur. Okurken sayfaları
yakınlaştırıp bir bölgeyi işaretleyerek kitabın "Favoriler" listesine ekleyebilirsin.
Her şey telefonun kendi tarayıcı belleğinde (IndexedDB) saklanır — sunucuya hiçbir
fotoğraf gitmez. iPhone'da, sadece dikey konumda kullanılacak şekilde tasarlandı.

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
- **Kırpma alanı**: dört köşedeki tutamaçları sürükleyerek ayarla (artık kenarlardan
  yeterince içeride başlıyor, iOS'un kenar kaydırma hareketiyle çakışmıyor).
- **Döndür**: üstteki döngü ikonuna her basışta 90° döner.
- **Aynala**: yatay olarak ters çevirir.
- **Sıfırla**: tüm düzenlemeleri (döndürme, aynalama, yakınlaştırma, AI hizalama) geri alır.
- **✨ Yapay zeka ile hizala**: API anahtarı gerektirir (bkz. aşağıda), fotoğrafı
  analiz ettirip sayfayı otomatik düzeltir ve kırpma alanını önerir. Bu, ince açı
  düzeltme + otomatik kırpma yapar — tam "keystone" (dört köşeden perspektif)
  düzeltmesi değildir, elle çekilmiş hafif eğik fotoğraflar için düşünülmüştür.
- Filtreler: Orijinal / Siyah-Beyaz Tarama / Netleştir.

## Yapay zeka ile otomatik hizalama (API anahtarı)

Kütüphanem'in sağ üstündeki dişli ikonundan **Ayarlar**'ı aç, kendi Anthropic API
anahtarını yapıştır. Anahtarını almak için:

1. [platform.claude.com](https://platform.claude.com) adresine git, hesap oluştur/giriş yap.
2. **Settings → API keys → Create key**.
3. `sk-ant-` ile başlayan anahtarı kopyalayıp Kütüphanem'in ayarlarına yapıştır.

**Önemli:** Bu anahtar yalnızca telefonunun tarayıcısında (localStorage) saklanır ve
kırpma ekranındaki "AI ile hizala" butonuna bastığında doğrudan tarayıcından
Anthropic'in API'sine gönderilir — aradan bir sunucu geçmez. Bu, anahtarın cihazının
dışına çıkmadığı anlamına gelir, ama aynı zamanda tarayıcı geliştirici araçlarını açan
biri (örneğin ortak kullanılan bir cihazda) anahtarı görebilir demektir — kişisel,
tek kullanıcılı bir uygulama için makul bir denge, ama başkalarıyla paylaşılan bir
cihazda kullanmamaya dikkat et. Her istek Anthropic hesabından ücretlendirilir
(küçük görseller ve kısa yanıtlar kullanıldığı için maliyeti düşüktür).

## Sayfa yakınlaştırma ve işaretleme (Favoriler)

- Okurken bir sayfada **iki parmakla yakınlaştırıp** kaydırabilirsin; çift dokunuş da
  yakınlaştırıp geri çıkarır. Yakınlaştırılmışken kaydırma sayfa çevirmez, sadece
  görüntüyü kaydırır — sayfayı çevirmek için önce tekrar uzaklaştır.
- Alt çubuktaki **"İşaretle"** butonuna basıp sayfada bir bölgeyi sürükleyerek
  seç, istersen kısa bir not ekle, kaydet. Bu bölge o kitabın **Favoriler**
  listesine (⋮ menüsünden erişilir) küçük bir önizlemeyle eklenir; listeden bir
  öğeye dokunmak seni doğrudan o sayfaya götürür.

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
  tarayabilir, rafta hepsini bir arada tutabilirsin.



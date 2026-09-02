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
- **Sıfırla**: döndürme, aynalama, yakınlaştırma ve varsa uygulanmış perspektif
  düzeltmesini geri alıp orijinal fotoğrafa döner.
- **✨ Yapay zeka ile perspektif düzeltme**: API anahtarı gerektirir (bkz. aşağıda).
  Sayfayı karşıdan değil çaprazdan/açılı çektiğinde (sayfa fotoğrafta dörtgen/trapez
  gibi göründüğünde), AI sayfanın dört köşesini bulur ve fotoğrafı bu köşelere göre
  gerçek bir "keystone" (perspektif) düzeltmesiyle düzleştirir — sanki sayfa tam
  karşıdan çekilmiş gibi. Arka plandaki masa, çevredeki eşyalar vb. otomatik olarak
  dışarıda kalır, sadece sayfa kalır. Sonucu beğenmezsen kalan kenarları köşe
  tutamaçlarıyla elle inceltebilirsin.
- **✨ AI Temiz Tarama** (filtre seçeneklerinin arasında): AI fotoğrafı analiz edip
  sayfanın arka planını nerede beyaza, yazıyı nerede siyaha çekmesi gerektiğini
  önerir, bu değerlerle gerçek bir "levels" (ton aralığı) düzeltmesi uygulanır —
  sabit kontrast filtrelerinden daha isabetli bir temiz tarama görünümü verir.
  **Not:** Gemini görsel üretmiyor/yeniden çizmiyor; bu yüzden "yazıları
  ve şekilleri tamamen yeniden çizip temiz bir sayfa oluşturma" gibi bir işlem
  mümkün değil — burada yapılan, AI'nin bulduğu köşelere/parametrelere göre var
  olan fotoğrafı geometrik olarak düzleştirmek ve tonlarını temizlemek.
- Filtreler: Orijinal / Siyah-Beyaz Tarama / Netleştir / AI Temiz Tarama.

## Yazıyı okuma (OCR) — işaretlerken

Bir bölgeyi işaretleyip not eklerken, not kutusunun yanındaki **✨ butonuna**
basarsan, AI o bölgedeki yazıyı okuyup metne çevirir ve otomatik olarak not
alanına yazar. Böylece önemli bulduğun bir pasajı hem görsel olarak işaretlemiş
hem de metnini favoriler listesinde okunabilir/kopyalanabilir halde saklamış
olursun. Bu da aynı ayarlardaki API anahtarını kullanır.

## Yapay zeka ile otomatik hizalama (API anahtarı)

Kütüphanem'in sağ üstündeki dişli ikonundan **Ayarlar**'ı aç, kendi Google Gemini
API anahtarını yapıştır. Anahtarını almak için:

1. [aistudio.google.com/apikey](https://aistudio.google.com/apikey) adresine git, Google hesabınla giriş yap.
2. **Create API key**'e bas.
3. `AIza` ile başlayan anahtarı kopyalayıp Kütüphanem'in ayarlarına yapıştır. Ücretsiz
   katman (Google AI Studio) kredi kartı gerektirmeden yeterli kotayı sağlıyor.

**Önemli:** Bu anahtar yalnızca telefonunun tarayıcısında (localStorage) saklanır ve
kırpma ekranındaki AI özelliklerine bastığında doğrudan tarayıcından Google'ın
Gemini API'sine gönderilir — aradan bir sunucu geçmez. Bu, anahtarın cihazının
dışına çıkmadığı anlamına gelir, ama aynı zamanda tarayıcı geliştirici araçlarını açan
biri (örneğin ortak kullanılan bir cihazda) anahtarı görebilir demektir — kişisel,
tek kullanıcılı bir uygulama için makul bir denge, ama başkalarıyla paylaşılan bir
cihazda kullanmamaya dikkat et. Her istek Google hesabından ücretlendirilir/kotadan
düşer (küçük görseller ve kısa yanıtlar kullanıldığı için ücretsiz katman genelde
yeterlidir).

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



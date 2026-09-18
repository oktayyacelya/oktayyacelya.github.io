# Yelkovan web sitesi

Sade HTML + CSS. Derleme adımı yok, npm yok, framework yok, **JavaScript yok**.
Dosyaları düzenle, kaydet, tarayıcıda yenile. Hepsi bu.

Canlı adres: <https://oktayyacelya.github.io/> — domain alınınca `yelkovan.ai` olacak.

---

## Siteyi kendi bilgisayarında görmek

```bash
cd ~/development/oktayyacelya.github.io
python3 -m http.server 8000
```

Sonra tarayıcıda <http://localhost:8000> adresini aç. Durdurmak için terminalde `Ctrl+C`.

> **Dosyaları asla çift tıklayarak açma.** `file://...` ile açtığında `/css/main.css`
> gibi yollar diskin köküne gider ve site bozuk görünür. Saatlerce sebebini ararsın.
> Site sadece yukarıdaki sunucu üzerinden görüntülenir.

## Değişikliği yayına alma

```bash
git add -A
git commit -m "Ne değiştirdiğini ve nedenini yaz"
git push
```

Yayınlanması 1-2 dakika sürer. Değişiklik görünmüyorsa `Cmd+Shift+R` ile sert yenile —
GitHub Pages CSS dosyalarını agresif önbelleğe alır.

---

## Sayfa haritası

| Türkçe | İngilizce |
|---|---|
| `/` | `/en/` |
| `/faby/` | `/en/faby/` |
| `/sorula/` | `/en/sorula/` |
| `/hakkimizda/` | `/en/about/` |
| `/iletisim/` | `/en/contact/` |
| `/gizlilik/` | `/en/privacy/` |
| `/blog/` | `/en/blog/` |
| `/blog/tek-ekip-iki-uygulama/` | `/en/blog/one-team-two-apps/` |

Her sayfa kendi klasöründe `index.html` olarak durur. Böylece adreste `.html` görünmez.

**Kurallar:**
- Her iç link `/` ile başlar **ve** `/` ile biter.
- Her dosya adı küçük harf, ASCII, tire ile ayrılmış.
  (macOS büyük/küçük harf ayırmaz ama GitHub Pages ayırır — `Logo.png` bilgisayarında
  çalışır, canlıda 404 verir.)

---

## İki dil senkron nasıl kalır

Derleme adımı olmadığı için bunun sihirli çözümü yok. Kural basit:

1. **Tek dilde sayfa olmaz.** Türkçesini yazdıysan İngilizcesini de yaz.
2. **Bir commit = iki dil.** `git status`'ta tek dil görünüyorsa iş bitmemiştir.
3. **Metinler kısa kalsın** (sayfa başına ~350 kelime). Asıl senkron stratejisi bu:
   300 kelime beş dakikalık çeviri, 1500 kelime ertelenecek bir angarya.

**Yapı denetimi** — bir dilde bölüm ekleyip diğerinde unuttuysan burada görünür:

```bash
for pair in "index.html en/index.html" \
            "faby/index.html en/faby/index.html" \
            "sorula/index.html en/sorula/index.html" \
            "hakkimizda/index.html en/about/index.html" \
            "iletisim/index.html en/contact/index.html" \
            "gizlilik/index.html en/privacy/index.html"; do
  set -- $pair
  diff <(grep -o 'id="[^"]*"' "$1") <(grep -o 'id="[^"]*"' "$2") >/dev/null \
    && echo "OK  $1" || echo "FARKLI  $1 <-> $2"
done
```

---

## Menü ve alt bilgi 16 dosyada tekrarlanıyor

Bu bilinçli bir tercih. Menüyü JavaScript ile yerleştirmek Google'ın linkleri görmesini
zorlaştırır ve sayfa açılırken boş başlık çakması yaratır. Menü 4 linkten oluşuyor ve
nadiren değişecek.

Menüyü değiştirdiğinde:
1. `<!-- nav:start v1 -->` ile `<!-- nav:end -->` arasını **her dosyada** güncelle.
2. Sürüm numarasını artır: `v1` → `v2`.
3. Unuttuğun dosya kaldı mı diye bak:

```bash
grep -rl "nav:start v1" --include='*.html' .   # eski sürümde kalanlar
```

Menü bloğu bir dilde tüm sayfalarda birebir aynıdır. **İki istisna:**
- Bulunduğun sayfanın linkinde `aria-current="page"` bulunur.
- Dil değiştirme linki o sayfanın karşılığına gider (`/faby/` ↔ `/en/faby/`).
  Bu adres `<head>` içindeki `hreflang` etiketleriyle aynı olmalı.

---

## İddia sicili

**Sitede yazan her sayı bu listede olmalı.** Listede yoksa siteye girmez.

Serbest:
- 19 sınav takibi (Sorula)
- 7 dil (Faby)
- 2–12 yaş (Faby)
- 15 ücretsiz dünya klasiği (Faby)
- Çıkmış sınavlar 2018–2025 (Sorula)
- App Store 5,0 — 18 değerlendirme, **Eylül 2026 itibarıyla** (Faby)
- App Store 5,0 — 27 değerlendirme, **Eylül 2026 itibarıyla** (Sorula)
- İTÜ Çekirdek (Sorula)

**Yasak:** her türlü indirme/kullanıcı sayısı, "50.000+", "4.9/12500", "300K+ soru",
"en hızlı", "lider", "milyonlarca".

Her puan cümlesi "… itibarıyla" taşır. Böylece sayı eskidiğinde yanlış değil, tarihli olur.
Puanlar değiştiğinde güncellenecek yerler: `faby/index.html`, `en/faby/index.html`,
`sorula/index.html`, `en/sorula/index.html` — hepsinde `page-meta` satırı.

**Faby'ye özel iki kural** (uygulamanın kendi marka dokümanından):
- "Hızlı üretim" / "anında oluşur" iddiası yazılmaz.
- Pazarlamada gerçek çocuk fotoğrafı veya videosu kullanılmaz.

---

## İçerik sınırı

> **Yelkovan şirketi anlatır. `sorula.app` ve `fabyfun.com` ürünleri anlatır.**

Bu siteye **asla** girmeyecek içerik:
- Sınav sayfaları veya sınav tavsiyesi (TYT/AYT/LGS/KPSS adresleri `sorula.app`'in)
- Tam özellik listeleri, fiyatlandırma, masal örnekleri
- Ebeveynlik / çalışma rehberi yazıları (`fabyfun.com` blogunun)
- Kullanıcı destek içeriği

Sebebi: aynı konuyu iki sitede yazarsan Google'da kendi sitenle yarışırsın ve ikisi de düşer.

Aynı sebeple: **ürün sitelerine `canonical` verilmez.** Cross-domain canonical Google'a
"bu sayfa kopyadır, sil" demektir ve şirket siteni dizinden düşürür. Doğru sinyal, ana
sayfadaki `Organization` JSON-LD içindeki `sameAs` listesi ve normal linklerdir.

---

## Görsel eklemek

Kaynak görseller `~/Desktop/Ay Creations/` altındaki uygulama depolarında.
**O dosyalara asla dokunma** — başka bir hesaba ait canlı git depoları.
Hep `--out` ile buraya yeni dosya yaz:

```bash
sips -s format jpeg -s formatOptions 72 -Z 608  "<kaynak>.png" --out assets/faby/yeni.jpg
sips -s format jpeg -s formatOptions 72 -Z 1216 "<kaynak>.png" --out "assets/faby/yeni@2x.jpg"
```

Kurallar:
- Tek görsel ≤ 200 KB, sayfa toplamı ≤ 900 KB. `git add` **öncesi** `ls -lhS assets/**/*` ile bak.
  Büyük bir dosya bir kez commit edilirse git geçmişinde sonsuza kadar kalır.
- Her `<img>` etiketi `width`, `height` ve `alt` taşır. `width`/`height` yazmak sayfa
  kaymasını (layout shift) tamamen ortadan kaldırır — tek başına en çok işe yarayan alışkanlık.
- Boyutu öğrenmek için: `sips -g pixelWidth -g pixelHeight dosya.jpg`

Logo `assets/brand/logo.svg` — elle yazılmış SVG, ~1 KB. Rengini değiştirmek için
dosyadaki `#B4552F` (vurgu) ve `#A89680` (taç yapraklar) değerlerini değiştir.

---

## Renkleri ve yazıyı değiştirmek

Hepsi `css/tokens.css` içinde. Oradaki bir değeri değiştirdiğinde tüm sayfalar birden değişir.
Başka CSS dosyasına dokunman gerekmez.

CSS dosyaları:
- `main.css` — HTML'in bağlandığı tek dosya, diğerlerini `@import` eder
- `tokens.css` — değerler (renk, yazı, boşluk)
- `base.css` — tarayıcı düzeltmeleri, temel etiketler
- `layout.css` — yerleşim (`.container` `.section` `.grid` `.stack`)
- `components.css` — parçalar (`.site-header` `.card` `.btn` `.site-footer`)

---

## Yayına almadan önce yapılacaklar

Site şu an **arama motorlarına kapalı.** Her sayfada şu satır var:

```html
<meta name="robots" content="noindex">
```

Yayına hazır olduğunda:

1. **İletişim adresini düzelt.** Şu an `merhaba@yelkovan.ai` yazıyor ve bu adres
   **henüz çalışmıyor** (domain alınmadı). Gerçek adresle değiştir:
   ```bash
   grep -rl "merhaba@yelkovan.ai" --include='*.html' . | xargs sed -i '' 's/merhaba@yelkovan\.ai/GERÇEK@ADRES/g'
   ```
2. **İTÜ Çekirdek ifadesini kontrol et.** Şu an "yer aldı" yazıyor. Program hâlâ
   devam ediyorsa "yer alıyor" olmalı. Geçtiği yerler: `index.html`, `en/index.html`,
   `sorula/index.html`, `en/sorula/index.html`.
3. **noindex satırlarını kaldır** — blog hariç:
   ```bash
   grep -rl 'name="robots" content="noindex"' --include='*.html' . | grep -v blog | xargs sed -i '' '/name="robots" content="noindex"/d'
   ```
4. **Domain adresi.** Domain alınmadan yayınlıyorsan `yelkovan.ai` yazan tüm adresleri
   `oktayyacelya.github.io` ile değiştir:
   ```bash
   grep -rl "yelkovan.ai" --include='*.html' . | xargs sed -i '' 's|https://yelkovan\.ai|https://oktayyacelya.github.io|g'
   sed -i '' 's|https://yelkovan\.ai|https://oktayyacelya.github.io|g' sitemap.xml robots.txt
   ```

### Blogu açmak

Blog şu an gizli. Açmak için üç şey birden yapılır:
1. Blog sayfalarındaki `noindex` satırlarını sil
2. `robots.txt` içindeki iki `Disallow` satırını sil
3. `sitemap.xml` içine blog adreslerini ekle

Önce en az 2-3 yazı hazır olsun. Boş veya güncellenmeyen blog siteye zarar verir.

### Domain bağlamak (yelkovan.ai alındıktan sonra)

1. `.ai` uzantısı **en az 2 yıllık** kayıt ister, yıllık ~$70-110.
2. DNS ayarları: kök alan için dört `A` kaydı →
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   `www` için `CNAME` → `oktayyacelya.github.io`
3. GitHub → depo → Settings → Pages → Custom domain kutusuna `yelkovan.ai` yaz.
   **`CNAME` dosyasını elle oluşturma** — GitHub kendisi yazar. Domain hazır değilken
   elle oluşturursan github.io adresi anında ölür ve siteyi görecek tek yolunu kaybedersin.
4. `git pull` (GitHub az önce `CNAME` dosyasını commit etti)
5. Sertifika geldikten sonra "Enforce HTTPS" kutusunu işaretle
6. Yukarıdaki 4. maddeyi tersine çevir: adresler tekrar `yelkovan.ai` olsun
7. E-posta için Cloudflare Email Routing (ücretsiz yönlendirme) en ucuz yol

---

## Yayın öncesi kontrol listesi

```bash
# Kırık iç link
grep -rho 'href="/[^"#]*"' --include='*.html' . | sed 's/href="//;s/"$//' | sort -u | \
while read -r p; do case "$p" in */) t=".${p}index.html";; *) t=".${p}";; esac; [ -f "$t" ] || echo "KIRIK: $p"; done

# Kırık görsel
grep -rho 'src="/[^"]*"' --include='*.html' . | sed 's/src="//;s/"$//' | sort -u | \
while read -r p; do [ -f ".${p}" ] || echo "KIRIK: $p"; done

# Büyük harfli dosya adı — canlıda 404 sebebi
find . -name "*[A-Z]*" -not -path "./.git/*" -not -path "./.claude/*"

# .DS_Store sızmış mı
find . -name ".DS_Store" -not -path "./.git/*"

# Toplam görsel boyutu
du -sh assets/
```

Tarayıcıda: 320px genişlikte yatay kaydırma olmamalı · Türkçe karakterler doğru
basılmalı (`İstanbul`, `öğrenme`) · dil değiştirme iki yönde de doğru sayfaya gitmeli ·
mağaza linkleri doğru uygulamaya gitmeli.

---

## Bilinen eksikler

- **Logo geçici.** `assets/brand/logo.svg` — 12 çizgili "çiçek kadran" tasarımı.
  Küçük boyutlarda (16px favicon) taç yapraklar birbirine giriyor. Son hâli verilmedi.
- **İletişim adresi çalışmıyor** — yukarıdaki "Yayına almadan önce" bölümüne bak.
- **Depo herkese açık** (ücretsiz GitHub kullanıcı sitesi için zorunlu). İçine hiçbir
  şifre, API anahtarı veya özel bilgi koyma. Commit e-postan da herkese açıktır —
  bu depoda GitHub'ın gizlilik adresi kullanılıyor.

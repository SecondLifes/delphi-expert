# AI Görsel Prompt'ları — README Banner'ları

Bu kit'in `README.md` / `README.tr-TR.md`'si için üç banner görseli.
Herhangi bir yetenekli görsel modeliyle (Nano Banana Pro, Midjourney v7,
Flux, GPT-Image vb.) **geniş 16:9 banner oranında** üret, PNG olarak
`docs/images/` altına kaydet (`overview.png`, `core-features.png`,
`design-philosophy.png` — aşırı büyük çıktıyı önce
`tools/resize-images.bat` ile küçült). `README.md`/`README.tr-TR.md`'deki
görsel etiketleri zaten açık; dosyalar yerine iner inmez yeni resimler
eskilerinin yerini alır.

Bu dosya **kendi başına yeterlidir** — artık paylaşılan bir temel prompt
yok. Her spec-kit tamamen kendine ait bir görsel dünyaya sahiptir; bu kit
başka bir kitin paletini, merkez nesnesini veya metaforunu asla yeniden
kullanmaz.

> **Not:** Prompt metinlerinin kendisi bilinçli olarak İngilizce
> bırakıldı — görsel üretim modelleri İngilizce'de daha tutarlı sonuç
> veriyor.

## Sanat yönetimi — "Kahin'in Alevi"

Delphi adını antik Delphi tapınağından alır ve sembolü her zaman alev
olmuştur. Bu kitin felsefesi *"Yapısı Gereği Sıfır-Sızıntı"*dır —
ustaya emanet edilen hiçbir şey kaybolmaz. Görsel dil: **kutsal bir
alevin mühendislik hassasiyetiyle yandığı antik Yunan tapınak dünyası**,
epik ressamsı matte-painting olarak. Robot yok, maskot yok, bilgisayar
yok — metaforu taşıyan her şey dünyanın kendisi.

- **Dünya:** Delphi'nin uçurum kenarındaki kutsal alanı — mermer
  sütunlar, bronz sacayaklar, kazınmış steller, dağ alacakaranlığı.
- **Palet:** ay ışığında beyaz mermer ve alacakaranlık indigo-mor
  gökyüzüne karşı tek bir güçlü kırmızı-turuncu alev (Delphi'nin klasik
  alev kırmızısı).
- **Stil:** epik ressamsı matte-painting, dramatik doğal ışık, hacimsel
  alev ışıması, müze kalitesinde detay.
- **Tutarlılık:** üç görsel de aynı dünyayı ve paleti paylaşır; her biri
  farklı çekim tipi ve kamera açısı kullanır.

## Negatif Prompt (her üretime aynen yapıştır)

```
text, letters, readable words, logos, watermark, low quality, blurry,
humans, faces, gods, statues of people, robots, mascots, cartoon style,
computers, screens, keyboards, modern objects, daylight blue sky,
different art style between images
```

## Görsel 1 — Overview (`docs/images/overview.png`)

**Yeri:** README'nin en üstü, başlık/rozetlerin altı.
**Çekim:** vadinin karşısından geniş tanıtım planı.

**Prompt:**
```
An epic wide matte-painting of the ancient sanctuary of Delphi on a
mountain cliff at deep dusk: moonlit white marble columns of a circular
temple, and at its center a bronze tripod holding one powerful,
perfectly steady red-orange flame — the only warm light in the scene.
Sparks rise from the flame high into the indigo-purple sky and settle
into faint glowing constellations of abstract geometric glyphs, as if
the fire itself is writing among the stars. A single eagle circles far
above the temple. Mist in the valley below, dramatic scale, epic
painterly matte-painting, volumetric fire glow, wide 16:9 banner
composition, museum-quality detail.
```

## Görsel 2 — Core Features (`docs/images/core-features.png`)

**Yeri:** "Temel İlkeler" bölümünün başı.
**Çekim:** sunak yüksekliğinden orta, simetrik plan.

**Prompt:**
```
A medium, perfectly symmetrical shot of an ancient bronze tripod brazier
standing between two marble columns at dusk: its single red-orange flame
divides into four calm, steady tongues of fire, and each tongue cradles
a floating emblem forged of glowing bronze light — two interlocked
solid rings that cannot separate (nothing acquired is ever lost), a
balanced two-pan scale in perfect equilibrium (principled structure), a
laurel wreath closed into a complete circle (proven and verified work),
and a fluted column capital carrying weight (layered architecture).
Moonlit marble surfaces, indigo dusk behind, the four emblems clearly
distinct in silhouette and evenly spaced. Epic painterly matte-painting,
volumetric fire glow, wide 16:9 banner composition, museum-quality
detail.
```

## Görsel 3 — Design & Philosophy (`docs/images/design-philosophy.png`)

**Yeri:** "Tasarım & Felsefe" bölümünün başı.
**Çekim:** dramatik alçak açılı yakın plan, gece.

**Prompt:**
```
A dramatic low-angle close-up at night: a freshly carved marble stele
standing tall among weathered ancient ruins, its surface engraved with
an immaculate, unbroken geometric pattern of interlocking channels that
glow faintly red-orange from within — every joint of the pattern
flawless, no channel left open, no line left unfinished. A bronze
chisel and mallet rest at its base, work just completed. The sacred
flame burns out of focus in the background, its light reflecting off
the polished new marble while the old ruins stay dark — an old craft,
still building things that outlast their builders. Indigo night sky,
epic painterly matte-painting, volumetric fire glow, wide 16:9 banner
composition, museum-quality detail.
```

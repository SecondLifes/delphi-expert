# AI Görsel Prompt'ları — README Banner'ları

Bu kit'in `README.md` / `README.tr-TR.md`'si için üç banner görseli.
Herhangi bir yetenekli görsel modeliyle üret (Nano Banana Pro, Midjourney
v7, Flux, GPT-Image vb.), PNG olarak `docs/images/` altına kaydet (aşırı
büyük çıktıyı önce `tools/resize-images.bat` ile küçült), sonra
`README.md`/`README.tr-TR.md`'deki yerleştirme talimatlarını izle.

**Altın kurallar, maskot kavramı ve yapı:** bkz.
[`Prompts/system/image-generation-base-prompt.tr-TR.md`](system/image-generation-base-prompt.tr-TR.md)
— bu dosya sadece bunu bu kit için özelleştiriyor, tekrar yazmıyor.

> **Not:** Prompt metinlerinin kendisi bilinçli olarak İngilizce
> bırakıldı — görsel üretim modelleri İngilizce'de daha tutarlı sonuç
> veriyor.

## Bu kit'in özelleştirmesi

- **Maskot aksesuarı:** kafasında veya göğsünde küçük bir alev-şeklinde
  aksesuar (Delphi'nin klasik kırmızı alev logosuna gönderme).
- **Aksan rengi:** koyu kırmızı ve yanık turuncu, Delphi'nin klasik
  kırmızı alev logosunu çağrıştırır.
- **Çapa nesnesi (üç görselde de zorunlu):** klasik bir RAD-Studio/
  Delphi-tarzı IDE penceresi — mavimsi bir başlık çubuğu, düz ikon
  karolarından bir bileşen-paleti kenar çubuğu, parlayan amber-kırmızı
  monospace satırları olan bir kod-editörü paneli (okunaksız) ve düz
  iki-sütunlu satırlardan bir özellik-inceleyici paneli olan dikdörtgen
  bir pencere. Fabrika/dişli/boru arka planı yok — sadece sade bir masa.
- **Çekim çeşitliliği (bkz. master prompt'un Altın Kural 6'sı):** üç
  görselde sadece maskotun eylemini değil, çekim tipini ve kamera açısını
  da değiştir — aynı eylem aynı orta-plan çerçevelemeyle üç kez tekrar
  edilirse yine tek bir tekrarlanan görsel gibi okunur.

## Görsel 1 — Genel Bakış (Overview)

**Yer:** `README.md`'nin en üstü, başlık/rozetlerin hemen altı.
**Çekim:** geniş kuruluş çekimi — maskot kadrajda küçük-orta boyutta,
masa ve çevresi görünür.

**Prompt (İngilizce — değiştirmeyin):**
```
A wide establishing shot, charming illustration of a small builder-robot
mascot (a compact, rounded robot with a small flame-shaped accessory on
its head, glowing deep-red/burnt-orange joints on a dark graphite
chassis) sitting at a plain desk seen from a slight side angle, both
hands near a keyboard, facing a classic RAD-Studio/Delphi-style IDE
window floating just above the desk — a rectangular window with a
blue-tinted title bar, a component-palette sidebar of small plain icon
tiles along one edge, a code-editor pane with glowing amber-red monospace
lines (illegible, just code-like), and a property-inspector panel of
plain two-column rows on the other side. Beside the window, a small
glowing padlock/latch icon sits closed and secure, and a couple of folder
icons with visible corner-folds sit stacked neatly. The whole scene —
mascot, desk, and window — is visible with room around it. Plain dark
graphite background, no machinery or factory elements, warm red-orange
rim lighting. Cinematic wide composition, high production value, digital
illustration, detailed.
```

## Görsel 2 — Temel Özellikler (Core Features)

**Yer:** "Key Guidelines Taught to AI" bölümünün en üstü.
**Çekim:** orta hero çekimi — maskot ortalanmış, doğrudan kameraya
bakıyor, Görsel 1'den belirgin şekilde daha yakın.

**Prompt (İngilizce — değiştirmeyin):**
```
A medium hero shot, centered composition: the same small builder-robot
mascot (flame-shaped head accessory, glowing deep-red/burnt-orange
joints on a dark graphite chassis, consistent design with the other
images in this set), facing directly toward the camera at chest height,
the same classic RAD-Studio/Delphi-style IDE window (blue-tinted title
bar, component-palette sidebar, code-editor pane, property-inspector
panel) floating just behind one shoulder, presenting four distinct
glowing icons floating between itself and the viewer: two interlocking
geometric joints that never separate (zero-leak memory management); a
set of modular connected platforms (SOLID/dependency injection); a
pulsing circular feedback-loop icon (the TDD red-green-refactor cycle);
and a stack of glowing rectangular platforms (layered clean
architecture). Plain dark graphite background, no machinery or factory.
Deep red and burnt-orange, each icon clearly distinct from its neighbors
in silhouette. Cinematic, charming, digital illustration, detailed.
```

## Görsel 3 — Tasarım ve Felsefe: Zero-Leak by Construction

**Yer:** "Design & Philosophy" kapanış bölümünün en üstü.
**Çekim:** yakın çekim, dramatik diyagonal kompozisyon, alçak kamera
açısı — Görsel 1 ve 2'den belirgin şekilde daha sıkı ve daha alışılmadık
bir çerçeveleme.

**Prompt (İngilizce — değiştirmeyin):**
```
A tight close-up shot from a low, slightly diagonal camera angle: the
same small builder-robot mascot (flame-shaped head accessory, glowing
deep-red/burnt-orange joints on a dark graphite chassis, consistent
design with the other images in this set), its hands filling most of the
frame, carefully and precisely snapping two glowing interlocking
geometric joint-pieces together, checking closely that they lock with
zero gap between them, a small satisfied glow radiating outward the
instant they click into place — the same classic RAD-Studio/Delphi-style
IDE window glowing softly out of focus in the background. Dramatic close
framing, shallow depth of field. Plain dark graphite background, no
machinery or factory, warm red-orange lighting, a strong sense of
deliberate, careful construction. Cinematic, charming, digital
illustration, detailed.
```

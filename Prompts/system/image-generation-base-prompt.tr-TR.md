# Görsel Üretimi Master Prompt'u — README Banner'ları

Bir kit'in üç README banner görselini (Overview / Core Features / Design
& Philosophy) üretmenin standart kuralları ve yapısı. Bu workspace'teki
her `Prompts/image-prompts.md` / `Prompts/image-prompts.tr-TR.md` (root,
`blank-scaffold`, ve her `spec-kits/*`) bu dosyanın **paralel bir kopyası
değil, bir özelleştirmesi**dir — bu dosya evrensel kuralları ve yapıyı
sahiplenir; kit-başına dosya sadece maskotun stack'e özgü aksesuarını,
aksan rengini, çapa nesnesini ve üç sahnenin gerçek prompt metnini
sahiplenir.

Bu dosya, evrensel kuralların erken dönemde 5+ ayrı dosyaya neredeyse
birebir kopyalanmasından dolayı var — bir yerde öğrenilen bir ders
(maskot kavramı, fabrika/dişli arka planı yasağı, zorunlu çapa-nesnesi
kuralı), diğer her dosyaya elle tekrar uygulanmak zorunda kalıyordu. Bunu
burada bir kere güncelle; her kit-başına dosya onu tekrar yazmak yerine
buna referans verir.

## Neden soyut enerji değil de bir maskot

Buraya varılana kadar gözlemlenen iki başarısızlık modu:

1. **Saf soyut enerji** (parlayan kristaller/ışınlar/toplar) — kelimenin
   tam anlamıyla her şeyin reklamı olabilir. Bu workspace'in kendi root
   README banner'larında kanıtlandı.
2. **Tutarlı bir karakter olmadan "somut nesneler," ve yeterince tarif
   edilmemiş bir "atölye" ruh hâli** — model, kit'in gerçek konusuyla
   hiçbir ilgisi olmayan jenerik bir sci-fi robot-fabrikası sahnesine
   (dişliler, borular, yürüme platformları) varsayılan olarak düşer, ve
   "parlayan bir kod penceresi olan bir laptop" herhangi bir dil/araç
   olarak okunur, belirli bir tanesi değil. Bu workspace'in kendi
   `batch-script-expert` kit'inin ilk gerçek üretiminde kanıtlandı.

İşe yarayan: **tek, tutarlı bir maskot karakteri** — küçük, sevimli bir
robot — ve onu başka hiçbir stack'in makul biçimde gösteremeyeceği
**zorunlu, kit'e özgü bir çapa nesnesiyle** etkileşim hâlinde göstermek.
Docker'ın balinası ya da GitHub'ın Octocat'i ile aynı prensip: karakter,
üç görseli birbirine bağlayan ortak iplik; sahne ve çapa nesnesi, her
kit'in setini kesin biçimde *o* kit hakkında yapan şey.

## Altın kurallar (her kit-başına dosyada uygulanır, tekrar yazma — bu bölüme referans ver)

1. **Kit başına tek maskot tasarımı, üç görselde de sabit.** Küçük,
   sevimli, yuvarlak hatlı robot gövdesi. Kit'e özgü özelleştirme
   şununla sınırlı: (a) kit'in alanına bağlı küçük bir aksesuar (kırmızı
   markalı bir dil için bir alev, terminal-ağırlıklı bir kit için bir
   CRT-monitör kafa, bir denetim kit'i için bir prizma/mercek vb.), ve
   (b) aksan-renkli parlayan eklemler/ekran-parıltısı. Temel gövdeyi
   kit'e göre asla yeniden tasarlama.
2. **Üç görselin de her birinde zorunlu, kit'e özgü bir çapa nesnesi —
   ima edilmiş değil, jenerik bir laptop değil.** Sor: "bu tam nesne
   *farklı* bir stack'in kit'i için de mantıklı olur muydu?" Cevap
   evetse, yeterince spesifik değildir. Hâlihazırda kullanılan iyi
   örnekler: yanıp sönen alt çizgi imleçli klasik siyah bir Komut İstemi
   penceresi (`batch-script-expert`), bileşen-paleti kenar çubuklu bir
   RAD-Studio/Delphi-tarzı IDE penceresi (`delphi-expert`), bir
   belge/rapor paneline tutulan bir büyüteç (`prompt-analyzer-expert`).
3. **Sci-fi fabrika/dişli/boru/endüstriyel arka plan asla — kit'in gerçek
   konusu kelimenin tam anlamıyla bir fabrika olmadıkça.** Sıradan her
   kit için (bir dil/stack/alan uzmanı) sadece sade bir masa, tezgah veya
   ofis ortamı — bunu her prompt'ta açıkça ve olumsuz biçimde belirt
   (`no machinery or factory elements`); modeli fabrika klişesinden uzak
   tutmak için belirsiz bir "atölye" ruh hâli tarifine güvenme, işe
   yaramaz. **Tek doğrulanmış istisna bu workspace'in kendi root
   README'si** — `AI Spec-Kits Maker` gerçekten başka kit'leri döven bir
   fabrika, bu yüzden kendi üç görseli bilinçli olarak bir dövme/fabrika
   motifi taşır (bir tezgah, özdeş şablonlar üreten bir üretim bandı). Bu
   istisnayı, aynı derecede kelimenin tam anlamıyla bir gerekçe olmadan
   başka bir kit'e genelleme — "kulağa havalı geliyor" bir gerekçe
   değildir.
4. **Okunabilir metin/harf/logo/watermark YOK** — bir UI çerçeve ögesinin
   kendi yapısal şekli (bir başlık çubuğu, buton-şekilleri, düz satırlardan
   bir ızgara) hariç; bunlar ikonik, düz yazı değil. Başlığı asla eserin
   içine gömme; başlıklar görselin altında Markdown'da yaşar.
5. **Aksan rengi, bir kit'in kendi üç görselinde sabit kalır, ve zaten
   inşa edilmiş her diğer kit'ten görsel olarak ayrılmalıdır.** Yeni bir
   renk seçmeden önce her diğer `spec-kits/*/Prompts/image-prompts.md`'yi
   kontrol et. Şu anki talepler: `delphi-expert` = koyu kırmızı/yanık
   turuncu, `batch-script-expert` = siyah üzerine terminal yeşili,
   `prompt-analyzer-expert` = mor-camgöbeği prizma, bu workspace'in
   kendi root'u = grafit üzerine ergimiş altın/amber.
6. **Bir kit içindeki üç sahne arasında ayırt edicilik hâlâ zorunlu — ve
   sadece farklı bir *eylem* yeterli değil.** `batch-script-expert` için
   gerçek bir üretim üç farklı eylem kullandı (oturma, ayakta durma,
   eğilme) ama üçü de yine "aynı resim" gibi okundu — her seferinde aynı
   orta-plan ön-cepheden çerçeveleme, kameraya aynı uzaklık, aynı ışık
   açısı. Eylem VE kamera/kompozisyon ikisi de değişmeli:
   - **Görsel 1 (Overview):** geniş veya orta-geniş çekim, maskot
     kadrajda küçük-orta boyutta, çapa nesne net görünür ama kadrajı
     doldurmuyor — bir kuruluş/tanıtım çekimi.
   - **Görsel 2 (Core Features):** orta çekim, maskot kabaca ortalanmış
     ve kameraya bakıyor, dışa doğru sunum yapıyor — bir "hero/ürün
     çekimi" çerçevelemesi, Görsel 1'den belirgin şekilde daha yakın ve
     daha cepheden.
   - **Görsel 3 (Design & Philosophy):** yakın çekim veya dramatik bir
     merkez-dışı/diyagonal kompozisyon, diğer ikisinden daha sıkı bir
     kırpma, sıra dışı bir kamera açısı (yukarıdan, yandan, alçak açı) —
     farklı bir fotoğrafçının çektiği gibi görünmeli.
   Çekim tipini ve çerçevelemeyi prompt metninde açıkça belirt (ör.
   `wide establishing shot`, `medium hero shot, centered`, `close-up,
   dramatic diagonal composition, low camera angle`) — farklı sonuçlar
   zorlamak için sadece eylem tarifine güvenme.
7. **En-boy oranı 1280×640 (2:1), PNG, görünür watermark yok.**
8. **Temel negatif prompt — her kit-başına dosyanın kendi negatif
   prompt'u bunun tamamını içermeli, ekleme serbest, çıkarma değil:**
   `text, letters, words, logos, watermark, low quality, blurry,
   distorted hands, extra limbs, scary or menacing robot, photorealistic
   human, generic stock art, different mascot design between images,
   factory background, gears, pipes, industrial machinery, sci-fi
   facility`

## Kit-başına bir `Prompts/image-prompts.md` gerçekte neyi içermeli

Bu dosya evrensel kuralları sahiplendiğine göre, kit-başına dosya sadece
şunlara ihtiyaç duyar:

1. Altın kurallar için bu dosyaya geri işaret eden tek satırlık bir not
   (tekrar yazma).
2. Kit'in kendi maskot aksesuarı + aksan rengi (Altın Kural 1/5).
3. Kit'in kendi çapa nesnesi, somut biçimde adlandırılmış (Altın Kural 2).
4. Üç gerçek, doğrudan yapıştırılabilir prompt metni bloğu — bunlardan
   kaçınılamaz, çünkü bunlar bir görsel modeline gerçekten yapıştırılan
   şeyler — her görsel yeri için bir tane, o yer için spesifik
   sahneyi/eylemi adlandırarak (Altın Kural 6).

## Çıktı

Üretilen görseller her kit'in kendi `docs/images/` klasörü altına PNG
olarak kaydedilir (`overview.png`, `core-features.png`,
`design-philosophy.png`), aşırı büyükse `tools/resize-images.bat` ile
küçültülür, sonra o kit'in `README.md`/`README.tr-TR.md`'sindeki
yorumdan çıkarılmış placeholder'lara yerleştirilir.

# Claude'a Verilecek Prompt — 3D Endless Runner Oyunu

Aşağıdaki metni olduğu gibi kopyalayıp yeni bir Claude sohbetine yapıştırabilirsin.

---

## PROMPT BAŞLANGICI

Subway Surfers / Temple Run tarzında, tarayıcıda çalışan 3D bir "endless runner" (sonsuz koşu) oyunu geliştirmeni istiyorum. Oyun ailem ve arkadaşlarımla oynayacağım eğlenceli, viral olma potansiyeli olan basit bir oyun olmalı. Lütfen aşağıdaki tüm gereksinimleri eksiksiz karşıla.

### 1. Teknik Zorunluluklar
- **Tek dosya**: Tüm kod (HTML + CSS + JavaScript) **tek bir .html dosyasında** olmalı. Harici dosya bağımlılığı olmamalı (sadece CDN üzerinden Three.js gibi kütüphaneler `<script>` etiketiyle çekilebilir).
- **3D render motoru**: Three.js kullan (CDN üzerinden yükle, örn. `https://cdnjs.cloudflare.com/ajax/libs/three.js/...`). Unity/Unreal gibi harici bir motor kullanılmayacak.
- **Asset stratejisi**: Gerçek 3D model/texture dosyaları (PolyHaven vb.) tek dosyalık bir HTML'e gömülemeyeceği için, tüm görseller **prosedürel olarak Three.js geometrileri, materyaller ve basit renk/gradient dokularıyla** oluşturulsun (low-poly / stilize bir görünüm hedeflensin). Gerekirse basit CSS gradient'leri veya canvas ile üretilmiş texture'lar kullanılabilir. Harici asset indirme/network isteği gerektiren hiçbir şey kullanma.
- **Performans**: Mobil cihazlarda dahi 60 FPS hedeflensin. Draw call sayısı düşük tutulsun, gereksiz post-processing kullanılmasın.
- **Uyumluluk**: Hem masaüstü (klavye) hem mobil (dokunmatik) tarayıcılarda düzgün çalışmalı. Ekran boyutuna göre otomatik responsive olmalı (resize event handling dahil).
- **Bağımsız çalışma**: İnternet olmadan da (Three.js CDN önbelleğe alındıysa) çalışabilecek şekilde, kod harici bir sunucu/backend gerektirmemeli. Skor vb. veriler `localStorage` ile saklanabilir.

### 2. Oynanış Mekanikleri
- Karakter otomatik olarak ileri koşar, oyuncu sadece yön/aksiyon kontrolü yapar.
- **3 şeritli** bir yol sistemi: sol / orta / sağ şerit arasında geçiş.
- Temel hareketler:
  - Şerit değiştirme (sola/sağa kaydırma)
  - Zıplama (engellerin üzerinden atlama)
  - Eğilme/kayma (engellerin altından geçme)
- Zamanla **hız kademeli olarak artmalı** (zorluk eğrisi), oyunu tekrar oynanabilir kılsın.
- Rastgele üretilen (procedural) engeller, platformlar ve boşluklar; segment tabanlı bir seviye üretim sistemi (chunk/tile spawn) kullan, sonsuz olacak şekilde.
- **Toplanabilir objeler** (coin/madeni para gibi) ekle, skor sistemine dahil et.
- **Power-up** örnekleri: mıknatıs (coinleri otomatik toplama), kalkan/zırh (bir çarpışmayı absorbe eder), hız düşürücü/yavaşlatıcı, çift puan.
- Çarpışma algılama (collision detection) basit bounding box / mesafe tabanlı olsun, performanstan ödün vermesin.
- Oyun bitince (çarpışma sonrası) skor ekranı ve "tekrar oyna" butonu göster.

### 3. Kontroller
- **Masaüstü**: Ok tuşları veya A/D (sol-sağ), W/Space (zıplama), S (eğilme).
- **Mobil**: Dokunmatik kaydırma (swipe) hareketleri — sağa/sola kaydırma şerit değiştirsin, yukarı kaydırma zıplasın, aşağı kaydırma eğilsin. Swipe algılama hassasiyeti ayarlanabilir olsun.
- Kontroller net şekilde ekranda küçük bir ipucu/tutorial ile ilk açılışta gösterilsin.

### 4. Görsel ve Tema
- Basit, renkli, "low-poly" / stilize bir görsel tarz (karmaşık gerçekçi grafik gerekmiyor — performans ve geliştirme kolaylığı öncelikli).
- Karakter basit bir 3D humanoid/blok figür olabilir (capsule + küp kombinasyonları gibi).
- Arka plan: gökyüzü (basit gradient/skybox), yol dokusu, iki yanda dekoratif nesneler (ağaç, bina, direk vb. basit geometrilerle).
- Gündüz/gece gibi basit bir renk teması, isteğe bağlı olarak zamanla değişen atmosfer (skybox rengi) eklenebilir.
- Basit parçacık efektleri (coin toplama, çarpışma) eklenebilir, ama performansı etkilemeyecek ölçüde.

### 5. Kullanıcı Arayüzü (UI)
- Ana menü ekranı: "Oyna" butonu, basit başlık/logo.
- Oyun içi HUD: anlık skor, en yüksek skor (localStorage'da saklı), toplanan coin sayısı.
- Duraklat (pause) butonu.
- Oyun bitti ekranı: final skor, en iyi skor, "Tekrar Oyna" ve (opsiyonel) skoru paylaşma metni.
- Tüm arayüz mobilde de rahat dokunulabilir (büyük butonlar, güvenli alan/safe-area desteği).

### 6. Ses (opsiyonel ama tercih edilir)
- Basit efekt sesleri (coin toplama, çarpışma, zıplama) Web Audio API ile **kod içinde üretilen** (oscillator tabanlı, dosya gerektirmeyen) seslerle eklenebilir. Harici ses dosyası kullanma.
- Sesi açma/kapama butonu ekle.

### 7. Kod Kalitesi
- Kod okunabilir ve yorum satırlarıyla (comment) açıklanmış olsun; ileride üzerine ekleme yapabilmem için mantıklı fonksiyonlara bölünmüş olsun (örn: `spawnObstacle()`, `updatePlayer()`, `checkCollision()` gibi).
- Global değişken kirliliğini önlemek için kodu bir modül/IIFE veya class yapısı içinde organize et.
- Oyunun tamamı tek `<html>` dosyası olarak teslim edilsin, doğrudan tarayıcıda çift tıklayarak açılabilsin.

### 8. Genişletilebilirlik (bonus, zorunlu değil)
- Farklı karakter/skin seçimi için basit bir altyapı.
- Görev/başarım (achievement) sistemi için genişletilebilir bir yapı.
- Kolayca yeni engel türleri eklenebilecek bir sistem (obstacle tanımlarının bir dizi/config üzerinden yönetilmesi gibi).

---

Lütfen yukarıdaki gereksinimlerin tamamını karşılayan, çalışır durumda, tek dosyalık bir HTML oyunu oluştur. Kodu adım adım değil, eksiksiz ve çalışır şekilde tek seferde ver.

## PROMPT SONU

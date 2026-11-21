# Tile Bloom — Tasarım Deposu (Design)

**Tile Bloom v1.0.1** görsel kimliğinin tek kaynağı. Google Stitch'e hazır.

## İçerik

| Dosya | Amaç |
| --- | --- |
| `DESIGN.md` | Açık DESIGN.md standardında tam tasarım spec'i — Stitch'in "Paste existing DESIGN.md" alanına yapıştır |
| `design-tokens.json` | Makine tarafından okunabilir token'lar (renk, tipografi, layout, motion) |
| `assets/app-icon.svg` | Ana uygulama ikonu (1024/512/192/180 PNG export'ları `assets/` içinde) |
| `screens/*.svg` | 390×844 ekran mockup'ları: menü, oynanış, seviye-tamamlandı, kaybetme |

## Google Stitch ile Kullanım

1. Google Stitch'i aç → **"Start with your design"** ekranı.
2. **"Paste existing DESIGN.md"** alanına `DESIGN.md` dosyasının tüm içeriğini yapıştır.
3. **"Upload code, images and logos"** kartına `assets/app-icon.svg` (+ PNG'ler) ve `screens/` klasöründeki SVG'leri sürükle.
4. Ek notlar alanına istersen ekleyebilirsin: *"Keep the glass-and-neon tile style; mascot must stay original; do not add stock imagery."*
5. Generate.

## Versiyon Kuralı

Bu depodaki versiyon (`DESIGN.md` altında) **1.0.1**. Oyun depolarıyla eşit tutulur;
`tile-bloom-dev` içindeki `npm run version:check` kontrol eder.

## Yeniden Kullanım

Tüm görsel varlıklar (ikon glifleri, maskot, ikon, mockup'lar) bu projede sıfırdan
üretilmiştir — hazır/telifli görsel yoktur.

© 2026 Elchin Jabbarli. Tasarım dosyaları projeyle birlikte kullanılır.

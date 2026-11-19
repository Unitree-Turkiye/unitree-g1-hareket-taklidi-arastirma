<div align="center">

# Unitree G1 Hareket Taklidi Araştırması: BeyondMimic'ten SONIC'e İnsansı Robotta Hareket İzleme (Motion Tracking)

**BeyondMimic · SuperMimic · supermimic_vibed · InstantTrack · BlinkMimic · XMimic · MacMimic · mjlab · GEAR SONIC · ProtoMotions**

**Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org · [www.robotlar.org](https://www.robotlar.org)**

👉 **Unitree G1 ürün sayfası: [robotlar.org/tr/insansi-robotlar/unitree-g1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · G1 EDU için teklif: [robotlar.org/tr/fiyat/unitree-g1-edu-fiyat](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat)**

📖 **Türkçe rehber: [Unitree G1'de hareket taklidi: BeyondMimic'ten SONIC'e hareket izleme yöntemleri](https://www.robotlar.org/tr/rehber/unitree-g1-hareket-taklidi-motion-tracking-rehberi)**

[Zaman Çizgisi](#-zaman-çizgisi) · [Karşılaştırma](#-yöntemlerin-karşılaştırması) · [Nereden Başlanır](#-nereden-başlanır-dizüstünden-laboratuvara) · [Veri ve Lisans](#-veri-setleri-ve-lisansları) · [Gerçek G1](#-simülasyondan-gerçek-g1e) · [Kaynaklar](#-kaynaklar) · [Katkı](#-katkı)

</div>

Bu depo, insan hareketini Unitree G1 insansı robota taklit ettiren hareket izleme (motion tracking) yöntemlerinin Türkçe araştırma özetidir. Kod, model ya da veri içermez. Her projenin ne önerdiğini, hangi altyapıda çalıştığını, kaynağının bildirdiği donanım ve süreyi, gerçek G1'de doğrulanıp doğrulanmadığını ve lisansını tek yerde toplar. Bütün rakamlar ilgili makaleye ya da depoya aittir, bağlantıları [Kaynaklar](#-kaynaklar) bölümünde.

## 🧭 Hareket izleme nedir?

Hareket izlemede insan hareketi kaydedilir, robotun iskeletine yeniden hedeflenir (retargeting) ve simülasyonda eğitilen bir politika bu referansı robotun kendi motorlarıyla izlemeyi öğrenir. Ödül, robotun eklem ve gövde konumlarının referansa ne kadar yaklaştığından hesaplanır. Akış dört adımdan oluşur:

1. **Hareket verisi:** hareket yakalama kaydı (LAFAN1, AMASS, BONES-SEED) ya da videodan kestirilen hareket.
2. **Yeniden hedefleme:** insan iskeletinden G1 eklemlerine aktarım. NVIDIA tarafındaki araç için [SOMA Retargeter rehberimize](https://www.robotlar.org/tr/rehber/soma-retargeter-unitree-g1-hareket-aktarimi-rehberi) bakın.
3. **Politika eğitimi:** binlerce paralel robotla simülasyonda pekiştirmeli öğrenme ya da türevlenebilir dünya modeliyle gradyan tabanlı eğitim.
4. **Sınama:** önce başka bir fizik motorunda sim2sim, sonra gerçek robot.

NVIDIA, SONIC makalesinde hareket izlemeyi büyük ölçekte eğitilebilen bir görev olarak tanımlar. Her davranış için ayrı ödül yazmak yerine tek izleme hedefi kullanılır ve beceri çeşitliliği veriden gelir.

## 🕰️ Zaman çizgisi

Tarihler makalelerin arXiv ilk sürümlerinden ve depoların GitHub açılış tarihlerinden alındı.

| Tarih | Gelişme |
|---|---|
| 2020 | Ubisoft La Forge, LAFAN1 veri setini SIGGRAPH 2020 makalesiyle yayımladı |
| Eylül 2024 | NVIDIA, ProtoMotions deposunu açtı (bugün ProtoMotions 3) |
| Haziran 2025 | mjlab deposu açıldı, makalesi 29 Ocak 2026'da arXiv'de |
| 11 Ağustos 2025 | BeyondMimic makalesi (arXiv 2508.08241) ve hareket izleme kodu |
| 11 Kasım 2025 | GEAR SONIC makalesi (arXiv 2511.07820) |
| 16 Mart 2026 | BONES-SEED veri seti açıldı (142 binden fazla hareket, yaklaşık 288 saat) |
| 20 Ekim 2026 | SuperMimic makalesi (arXiv 2610.28480), CoRL 2026 sözlü sunum |
| 21 Ekim 2026 | supermimic_vibed (Chong Zhang, ETH Zürich) |
| 28 Ekim 2026 | ProtoMotions'ta G1 için Metatron yayımlandı |
| 29 Ekim 2026 | InstantTrack |
| 2 Kasım 2026 | BlinkMimic |
| 7 Kasım 2026 | XMimic deposu, makalesi (arXiv 2611.20452) 16 Kasım'da |
| 10 Kasım 2026 | SuperMimic resmi kodu HybridRobotics deposunda |
| 15 Kasım 2026 | MacMimic |

## 📊 Yöntemlerin karşılaştırması

Süre sütunu her projenin kendi ölçümüdür. Projeler farklı veri, hedef ve donanımla ölçüldüğü için satırlar arasında doğrudan hız karşılaştırması yapılamaz.

| Proje | Kurum / yazar | Yıl | Yöntemin özü | Altyapı | Donanım ve süre (kaynağa göre) | Robotta doğrulama | Lisans | Bağlantılar |
|---|---|---|---|---|---|---|---|---|
| **BeyondMimic** | UC Berkeley (Hybrid Robotics), Stanford | 2025 | Tek MDP ve ortak hiperparametrelerle hareket izleme, üzerine görev yönlendirmeli gizil difüzyon | Isaac Lab 2.1.0, RSL-RL PPO; kod artık mjlab'de | Robotta izleme çıkarımı CPU'da 1 ms altı | Gerçek G1 (makale ekleri) | Kod MIT | [Makale](https://arxiv.org/abs/2508.08241) · [Kod](https://github.com/HybridRobotics/whole_body_tracking) · [Dağıtım](https://github.com/HybridRobotics/motion_tracking_controller) |
| **GEAR SONIC** | NVIDIA (GEAR) | 2025 | Hareket izlemeyi ölçekleme: 700 saat hareket yakalama, 1,2M-42M parametre | Isaac Lab eğitimi, C++ ve TensorRT dağıtımı | 21 bin GPU saati | Gerçek G1, 124 sekans (NVIDIA) | Kod Apache-2.0, ağırlıklar NVIDIA Open Model License | [Makale](https://arxiv.org/abs/2511.07820) · [Kod](https://github.com/NVlabs/GR00T-WholeBodyControl) · [Model](https://huggingface.co/nvidia/GEAR-SONIC) |
| **ProtoMotions** | NVIDIA (NVlabs) | 2024 | Benzetilen insan ve insansılar için GPU hızlandırmalı eğitim çatısı, G1 genel izleme politikası | IsaacGym, IsaacLab, Newton, MuJoCo | AMASS: 4 A100 ile 12 saat; BONES-SEED: 24 A100 | Gerçek G1, sıfır atış aktarım (NVIDIA) | Kod Apache-2.0, modeller NVIDIA Open Model License | [Kod](https://github.com/NVlabs/ProtoMotions) |
| **mjlab** | Zakka, Liao, Yi, Le Lay, Sreenath, Abbeel | 2025 | Isaac Lab tarzı yönetici API'si ile MuJoCo Warp birleşimi | MuJoCo Warp | Eğitim NVIDIA GPU, macOS'ta değerlendirme | Eğitim çatısı, G1 izleme görevi hazır | Apache-2.0 | [Makale](https://arxiv.org/abs/2601.22074) · [Kod](https://github.com/mujocolab/mjlab) |
| **SuperMimic** | UC Berkeley | 2026 | Tüm hareketler için tek türevlenebilir dünya modeli, hayali açılımlar üzerinden geri yayılım | MuJoCo Warp, kod mjlab üzerinde | Tek H100 ile ~3 saat; sıfır hata için 2 GPU ile 9,8 saat | Gerçek G1 (makale) | Apache-2.0 | [Makale](https://arxiv.org/abs/2610.28480) · [Sayfa](https://supermimic.github.io) · [Kod](https://github.com/HybridRobotics/supermimic) |
| **supermimic_vibed** | Chong Zhang (ETH Zürich) | 2026 | SuperMimic'in bağımsız, resmi olmayan yeniden yazımı | Isaac Lab 3, Newton + MuJoCo-Warp çözücü | RTX PRO 6000 Blackwell ile ~0,95 saat | Simülasyon (40 LAFAN1 klibi) | MIT | [Kod](https://github.com/zita-ch/supermimic_vibed) |
| **InstantTrack** | Huy-Quang-Dao | 2026 | GPU döngü mühendisliği, EMA politika, Muon, hata odaklı sıfırlama | mjlab 1.6.0, MuJoCo Warp | RTX 5090 ile ~3 dakika | Simülasyon (40 LAFAN1 klibi) | MIT | [Kod](https://github.com/Huy-Quang-Dao/InstantTrack) |
| **BlinkMimic** | Fuyi Chen | 2026 | Yarı örtük dünya modeli, ayna çoğaltma, değerlendirme güdümlü sıfırlama | mjlab, MuJoCo Warp | RTX 4090 ile hedefe 2,9 dakika | Simülasyon (40 LAFAN1 klibi) | MIT | [Kod](https://github.com/fuyichen0725/BlinkMimic) |
| **XMimic** | Nico Bohlinger, Jan Peters | 2026 | BlinkMimic algoritması JAX ile, her iterasyon tek XLA programı | JAX, MJX üzerinden MuJoCo Warp | 4 H100 ile hedefe 0,86 dakika | Simülasyon (40 LAFAN1 klibi) | MIT | [Makale](https://arxiv.org/abs/2611.20452) · [Kod](https://github.com/nico-bohlinger/xmimic) |
| **MacMimic** | air23zj | 2026 | BlinkMimic algoritması ve XMimic'in simülatör/öğrenici örtüşmesi, Apple silicon | CPU'da MuJoCo, GPU'da MLX | MacBook Pro M4 Pro ile hedefe ortalama 21,6 dakika | Simülasyon (40 LAFAN1 klibi) | MIT | [Sayfa](https://air23zj.github.io/macmimic/) · [Kod](https://github.com/air23zj/macmimic) |

## ⚡ SuperMimic ve türevleri

Klasik yaklaşımda politika simülatörde PPO gibi bir yöntemle deneme yanılma yaparak gelişir. SuperMimic ise simülatörden toplanan geçişlerle robotun ortak dinamiğini öğrenen tek bir türevlenebilir dünya modeli kurar. Politika bu modelin içinde hayal edilen kısa açılımlar boyunca zaman içinde geri yayılımla (BPTT) analitik gradyan alır.

Makaleye ve proje sayfasına göre SuperMimic, BONES-SEED'den süzülen 129.514 hareketle eğitildi ve tek H100'de yaklaşık 3 saatte SONIC'in normalleştirilmiş getirisini geçti. Yazarlar SONIC tabanının 128 GPU ile 7 gün eğitildiğini ve GPU saatinde 1000 kattan fazla azalma elde ettiklerini bildiriyor. 129.514 hareketin 25'inde hata kaldı (taban 179), iki GPU ile 9,8 saatlik eğitimden sonra hata sayısı sıfıra indi.

Açık kaynak türevler 40 klipli LAFAN1 kıyas setini kullanır. Ortak hedef normalleştirilmiş getirinin 0,82'ye ulaşması ve hiçbir klibin başarısız olmamasıdır.

| Depo | Neyi değiştirdi | Kaynağın bildirdiği sonuç |
|---|---|---|
| supermimic_vibed | SuperMimic'in bağımsız yeniden yazımı; README kodun bir kodlama asistanıyla bir saatte yazıldığını ve gözden geçirilmediğini belirtir | 0,842 getiri, 0/40 başarısız klip |
| InstantTrack | supermimic_vibed'in mjlab sürümü üzerine GPU döngü mühendisliği ve algoritma değişiklikleri | ~3 dakika (5 tohum); yazarın aynı makinedeki SuperMimic yeniden yazımı 1,50 saat |
| BlinkMimic | InstantTrack ve supermimic_vibed birleşimi | 3 tohum ortalamasıyla 2,9 dakika; standart koşu 3500 iterasyon, ~8 dakika |
| XMimic | Yöntem aynı, makine farklı: JAX, XLA, 2-4 GPU | 4 GPU 0,86 dakika, tek GPU 2,47 dakika; aynı makalede engebeli zemin için XSoma |
| MacMimic | MuJoCo 4096 robotu CPU'da, dünya modeli ve politika MLX ile GPU'da | 3 tohum ortalaması 21,6 dakika; aynı Mac'teki BlinkMimic MLX portundan (44 dakika) 2,04 kat hızlı |

InstantTrack, XMimic ve supermimic_vibed README'leri SuperMimic yazarlarıyla bağlantıları olmadığını belirtir. MacMimic karşılaştırması orijinal CUDA koduyla değil depodaki MLX portlarıyla yapıldı ve README'ye göre BlinkMimic hedefte biraz daha yüksek hassasiyete ulaşıyor (0,847'ye karşı 0,822).

## 🚀 Nereden başlanır: dizüstünden laboratuvara

| Düzey | Donanım (kaynağa göre) | Başlangıç projesi | Ne elde edersiniz |
|---|---|---|---|
| Dizüstünde simülasyon | MacBook Pro, M4 Pro, 48 GB | [MacMimic](https://github.com/air23zj/macmimic) | 40 LAFAN1 klibini izleyen G1 politikası; tam koşu yaklaşık 30 dakika |
| Tek tüketici GPU'su | NVIDIA RTX 4090 / RTX 5090 | [BlinkMimic](https://github.com/fuyichen0725/BlinkMimic), [InstantTrack](https://github.com/Huy-Quang-Dao/InstantTrack) ([mjlab](https://github.com/mujocolab/mjlab) üzerinde) | Aynı kıyas setinde dakikalar içinde hedef |
| İş istasyonu GPU'su | RTX PRO 6000 Blackwell ya da tek H100 | [supermimic_vibed](https://github.com/zita-ch/supermimic_vibed), [SuperMimic](https://github.com/HybridRobotics/supermimic) | Dünya modeli yöntemini büyük veriye taşıma |
| Çok GPU'lu sunucu | 4 H100 ya da 4-24 A100 | [XMimic](https://github.com/nico-bohlinger/xmimic), [ProtoMotions](https://github.com/NVlabs/ProtoMotions) | Dakika altı LAFAN1 eğitimi ya da AMASS ve BONES-SEED ölçeği |
| Büyük ölçek ve gerçek G1 | GPU kümesi + [Unitree G1 EDU](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) | [GEAR SONIC](https://github.com/NVlabs/GR00T-WholeBodyControl) | Gerçek G1'de tüm gövde kontrolü ve teleoperasyon |

mjlab macOS'u değerlendirme için destekler, eğitim için NVIDIA GPU ister. Gerçek robot aşamasına geçmeden önce politikayı MuJoCo'da sim2sim sınamasından geçirin.

## 📦 Veri setleri ve lisansları

| Veri | İçerik | Lisans | Kullanan projeler |
|---|---|---|---|
| [LAFAN1](https://github.com/ubisoft/ubisoft-laforge-animation-dataset) (Ubisoft La Forge) | 5 oyuncu, 77 sekans, 30 fps'de yaklaşık 4,6 saat | CC BY-NC-ND 4.0 (ticari olmayan, türev yasak) | BeyondMimic, supermimic_vibed, InstantTrack, BlinkMimic, XMimic, MacMimic |
| [LAFAN1_Retargeting_Dataset](https://huggingface.co/datasets/lvhaidong/LAFAN1_Retargeting_Dataset) (Hugging Face) | G1, H1 ve H1_2 için yeniden hedeflenmiş hareketler | Veri kartı LAFAN1 lisansını belirtir, kod MIT | Aynı projeler |
| [AMASS](https://amass.is.tue.mpg.de/license.html) (Max Planck Enstitüsü) | Birleştirilmiş hareket yakalama arşivi | Ticari olmayan bilimsel araştırma lisansı, yeniden dağıtım yasak | ProtoMotions |
| [BONES-SEED](https://huggingface.co/datasets/bones-studio/seed) (Bones Studio) | 142 binden fazla hareket, yaklaşık 288 saat, SOMA ve G1 biçimleri | BONES-SEED License: akademi ve yıllık geliri 1 milyon USD altındaki girişimler; öteki kurumlar için ayrı ticari lisans | GEAR SONIC, SuperMimic, ProtoMotions |

LAFAN1 kullanan depolar veriyi depoya koymaz, kurulumda indirir. Bu veriden türeyen kontrol noktaları ve videolar MIT kapsamı dışındadır ve ticari olmayan araştırma içindir. G1 robot tanımı BSD-3-Clause lisanslıdır. BONES-SEED maddelerinin Türkçe açıklaması [BONES-SEED rehberimizde](https://www.robotlar.org/tr/rehber/bones-seed-hareket-veri-seti-rehberi).

## 🔁 Simülasyondan gerçek G1'e

Gerçek G1 sonucu yayımlayan akışlar:

- **BeyondMimic:** [motion_tracking_controller](https://github.com/HybridRobotics/motion_tracking_controller) ROS 2 Jazzy ve legged_control2 üzerinde, ONNX politikasını CPU'da çalıştıran C++ hattıdır. MuJoCo sim2sim ve Ethernet üzerinden gerçek robot başlatıcısı aynı pakettedir. Politika Unitree kumandasıyla başlar, B tuşu acil durdurmadır.
- **GEAR SONIC:** C++ ve TensorRT dağıtımı; G1 üstündeki Jetson Orin için JetPack 6 ve TensorRT 10.7. Türkçesi: [GEAR SONIC rehberi](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi) · [unitree-g1-gear-sonic-turkce](https://github.com/Unitree-Turkiye/unitree-g1-gear-sonic-turkce).
- **ProtoMotions:** BONES-SEED ile eğitilen izleyici ONNX'e aktarılır, MuJoCo'da sınanır ve RoboJuDo ile gerçek G1'e bağlanır. Türkçesi: [ProtoMotions rehberi](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi) · [nvidia-protomotions-turkce](https://github.com/Unitree-Turkiye/nvidia-protomotions-turkce).

Yalnız simülasyonda sonuç veren depolarda robot aşaması kullanıcıya kalır. MacMimic README'si politikanın fiziksel bir robotta çalıştırılmadığını açıkça yazar. Böyle bir politikayı robota taşımadan önce MuJoCo'da sim2sim sınaması yapın ve robotu askı ya da destekle koruyun. Unitree'nin pekiştirmeli öğrenme akışı için [RL Gym sayfası](https://www.robotlar.org/tr/rl-gym) ve [unitree-rl-gym-turkce](https://github.com/Unitree-Turkiye/unitree-rl-gym-turkce), simülatör için [unitree-mujoco-turkce](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce) depolarımıza bakabilirsiniz.

## 🏢 Türkiye'de Unitree G1 ile hareket taklidi çalışmaları

Robotlar.org, Unitree Robotics Türkiye Resmi Distribütörü olarak G1 ailesiyle araştırma yapan üniversite ve şirket ekipleriyle çalışıyor. Hareket taklidi çalışmasının hangi düzeyde başlayacağını keşif görüşmesinde sizinle birlikte belirliyoruz: kimi ekip dizüstünde yöntemi anlamakla başlar, kimi doğrudan G1 EDU ile gerçek robotta sim2real çalışmasına geçer.

| Ürün | Hareket taklidi çalışmasındaki yeri |
|---|---|
| [Unitree G1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) | Bu depodaki yöntemlerin ortak deney gövdesi olan insansı robot |
| [Unitree G1 EDU teklif](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) | Geliştirici ve araştırma sürümü için teklif formu |
| [G1 Ar-Ge platformu](https://www.robotlar.org/tr/g1-arge-platformu) | Üniversite ve Ar-Ge laboratuvarları için G1 çözüm sayfası |
| [Unitree G1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-d) | İnsansı robot veri toplama ve eğitim platformu, Jetson Orin |
| [Unitree eğitim çözümleri](https://www.robotlar.org/tr/unitree-egitim-cozumleri) | Ders, laboratuvar ve eğitim programları |

Yapılandırma ve fiyat için [G1 EDU teklif sayfasını](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) ya da [kurumsal teklif formunu](https://www.robotlar.org/tr/fiyat/unitree-kurumsal-teklif-al) kullanabilirsiniz.

## 🧬 NVIDIA İnsansı Hareket Ailesi (Türkçe)

NVIDIA'nın animasyon, robotik ve fiziksel yapay zeka için yayımladığı insansı hareket projeleri birbirini tamamlar: SOMA ortak gövde modelini, BONES-SEED veriyi, GEM videodan hareketi, Kimodo ve ARDY hareket üretimini, MotionBricks gerçek zamanlı hareket planlamayı, SOMA Retargeter insan hareketini Unitree G1'e aktarmayı, ProtoMotions fizik tabanlı eğitimi, GEAR SONIC de gerçek robotta tüm gövde kontrolünü üstlenir. Akışın bütününü [merkez rehberde](https://www.robotlar.org/tr/rehber/nvidia-insansi-hareket-ekosistemi-unitree-g1-rehberi) anlatıyoruz.

| Proje | Ne yapar | Türkçe repo | Türkçe rehber | Resmi depo |
|---|---|---|---|---|
| MotionBricks | Bağlam ve hedef pozlar arasında gerçek zamanlı hareket üretir, Unitree G1 modeliyle gelir | [unitree-g1-motionbricks-turkce](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/unitree-g1-motionbricks-rehberi) | [NVlabs/motionbricks](https://github.com/NVlabs/motionbricks) |
| Kimodo | Denetlenebilir çevrim dışı insan hareketi üretimi | [nvidia-kimodo-turkce](https://github.com/Unitree-Turkiye/nvidia-kimodo-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-kimodo-hareket-uretimi-rehberi) | [nv-tlabs/kimodo](https://github.com/nv-tlabs/kimodo) |
| ARDY | Kimodo üzerine gerçek zamanlı, etkileşimli hareket üretimi | [nvidia-ardy-turkce](https://github.com/Unitree-Turkiye/nvidia-ardy-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-ardy-gercek-zamanli-hareket-rehberi) | [nv-tlabs/ardy](https://github.com/nv-tlabs/ardy) |
| SOMA Body Model | Birleşik, parametrik insan gövde modeli | [nvidia-soma-govde-modeli-turkce](https://github.com/Unitree-Turkiye/nvidia-soma-govde-modeli-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-soma-govde-modeli-rehberi) | [NVlabs/SOMA-X](https://github.com/NVlabs/SOMA-X) |
| BONES-SEED | SOMA ve G1 biçiminde büyük hareket yakalama veri seti | [bones-seed-hareket-veri-seti-turkce](https://github.com/Unitree-Turkiye/bones-seed-hareket-veri-seti-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/bones-seed-hareket-veri-seti-rehberi) | [bones-studio/seed](https://huggingface.co/datasets/bones-studio/seed) |
| ProtoMotions | Benzetilen insan ve insansıları eğitmek için GPU hızlandırmalı simülasyon çatısı | [nvidia-protomotions-turkce](https://github.com/Unitree-Turkiye/nvidia-protomotions-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi) | [NVlabs/ProtoMotions](https://github.com/NVlabs/ProtoMotions) |
| SOMA Retargeter | SOMA hareketini Unitree G1 gibi insansı robotlara aktarır | [unitree-g1-soma-retargeter-turkce](https://github.com/Unitree-Turkiye/unitree-g1-soma-retargeter-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/soma-retargeter-unitree-g1-hareket-aktarimi-rehberi) | [NVIDIA/soma-retargeter](https://github.com/NVIDIA/soma-retargeter) |
| GEM | Görüntü ve videodan tüm gövde ve el hareketini kestirir | [nvidia-gem-x-turkce](https://github.com/Unitree-Turkiye/nvidia-gem-x-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/nvidia-gem-x-videodan-hareket-rehberi) | [NVlabs/GEM-X](https://github.com/NVlabs/GEM-X) |
| GEAR SONIC | Fiziksel insansı robotlar için davranış temel modeli ve tüm gövde kontrolü | [unitree-g1-gear-sonic-turkce](https://github.com/Unitree-Turkiye/unitree-g1-gear-sonic-turkce) | [Rehber](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi) | [NVlabs/GR00T-WholeBodyControl](https://github.com/NVlabs/GR00T-WholeBodyControl) |

İlgili Türkçe kaynaklar: [Unitree G1 + Isaac GR00T](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) · [XR teleoperasyon](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [LeRobot](https://github.com/Unitree-Turkiye/unitree-lerobot-turkce) · [RL Gym](https://github.com/Unitree-Turkiye/unitree-rl-gym-turkce) · [MuJoCo](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce)

## 🤖 Unitree Ürün Ailesi (Türkiye)

Robotlar.org, Unitree Robotics'in Türkiye resmi distribütörüdür. Aşağıdaki ürünlerin Türkçe tanıtımı, teknik özellikleri ve kurumsal teklif formu sitede yer alır. Fiyatlar konfigürasyona göre değiştiği için [teklif üzerinden](https://www.robotlar.org/tr/fiyat/unitree-kurumsal-teklif-al) paylaşılır.

### İnsansı robotlar

| Ürün | Ne işe yarar |
|---|---|
| [Unitree G1](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) | 132 cm, 35 kg, 23-43 DoF insansı robot |
| [Unitree G1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-d) | İnsansı robot veri toplama ve eğitim platformu, Jetson Orin |
| [Unitree G1 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1-plus) | 2 DoF boyun, yüksek tork, dokunsal kafa ve 6 mikrofonlu G1 |
| [Unitree H2](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2) | 182 cm, 70 kg, 31 DoF tam boy insansı robot |
| [Unitree H2-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-d) | 7 DoF kollu, tekerlekli çift kollu insansı robot |
| [Unitree H2 Plus](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-plus) | NVIDIA Isaac GR00T referans insansı robotu, 75 DoF, Jetson Thor |
| [Unitree H2-X](https://www.robotlar.org/tr/insansi-robotlar/unitree-h2-x) | Hizmet ve koruma görevleri, 4 m platformdan iniş |
| [Unitree R1](https://www.robotlar.org/tr/insansi-robotlar/unitree-r1) | 123 cm, 29 kg, 26 DoF insansı robot (AIR, R1, EDU sürümleri) |
| [Unitree R1-D](https://www.robotlar.org/tr/insansi-robotlar/unitree-r1-d) | R1-A5 ya da R1-A7 (7-DOF) kollu çift kollu insansı platform |
| [Tüm insansı robotlar →](https://www.robotlar.org/tr/insansi-robotlar) | |

### Robot elleri

| Ürün | Ne işe yarar |
|---|---|
| [Unitree Dex3-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex3-1) | G1 için 7 serbestlik dereceli el, opsiyonel 33 dokunsal sensör |
| [Unitree Dex5-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex5-1) | 20 serbestlik dereceli (16 aktif) el, Dex5-1P sürümünde 94 dokunsal sensör |
| [Unitree Dex5-S](https://www.robotlar.org/tr/robot-elleri/unitree-dex5-s) | 22 aktif serbestlik, 620 g, çift T1s Ethernet, 1000 Hz |
| [Unitree Dex1-1](https://www.robotlar.org/tr/robot-elleri/unitree-dex1-1) | İki çeneli tutucu: 90 mm strok, 5-120 N ayarlı kavrama, RS-485 |
| [Tüm robot elleri →](https://www.robotlar.org/tr/robot-elleri) | |

### Dört ayaklı robotlar ve bileşenler

[Go2](https://www.robotlar.org/tr/kopek-robotlar/unitree-go2) · [Go2-W](https://www.robotlar.org/tr/kopek-robotlar/unitree-go2-w) · [B2](https://www.robotlar.org/tr/kopek-robotlar/unitree-b2) · [B2-W](https://www.robotlar.org/tr/kopek-robotlar/unitree-b2-w) · [A2](https://www.robotlar.org/tr/kopek-robotlar/unitree-a2) · [AS2](https://www.robotlar.org/tr/kopek-robotlar/unitree-as2) · [Tüm dört ayaklı robotlar](https://www.robotlar.org/tr/kopek-robotlar) · [4D LiDAR L2](https://www.robotlar.org/tr/unitree-bilesenler/4d-lidar-l2) · [Tüm Unitree bileşenleri](https://www.robotlar.org/tr/unitree-bilesenler)

## 📖 Türkçe Rehberler

- [NVIDIA insansı hareket ailesi ve Unitree G1 (merkez rehber)](https://www.robotlar.org/tr/rehber/nvidia-insansi-hareket-ekosistemi-unitree-g1-rehberi)
- **[Unitree G1'de hareket taklidi: BeyondMimic'ten SONIC'e hareket izleme yöntemleri](https://www.robotlar.org/tr/rehber/unitree-g1-hareket-taklidi-motion-tracking-rehberi)**
- [GEAR SONIC ile Unitree G1 tüm gövde kontrol rehberi](https://www.robotlar.org/tr/rehber/gear-sonic-unitree-g1-tum-govde-kontrol-rehberi)
- [NVIDIA ProtoMotions insansı simülasyon rehberi](https://www.robotlar.org/tr/rehber/nvidia-protomotions-insansi-simulasyon-rehberi)
- [SOMA Retargeter ile Unitree G1 hareket aktarımı](https://www.robotlar.org/tr/rehber/soma-retargeter-unitree-g1-hareket-aktarimi-rehberi)
- [BONES-SEED hareket veri seti rehberi](https://www.robotlar.org/tr/rehber/bones-seed-hareket-veri-seti-rehberi)
- [Unitree G1 Isaac GR00T rehberi](https://www.robotlar.org/tr/rehber/unitree-g1-isaac-gr00t-rehberi)
- [Unitree LeRobot: veri toplama ve taklit öğrenme](https://www.robotlar.org/tr/rehber/unitree-lerobot-rehberi)
- [Unitree G1 XR teleoperasyon](https://www.robotlar.org/unitree/g1-teleoperasyon)
- [Unitree RL Gym](https://www.robotlar.org/tr/rl-gym)
- [Unitree SDK rehberi](https://www.robotlar.org/unitree/sdk-rehberi)
- [Unitree eğitim çözümleri (üniversite ve laboratuvar)](https://www.robotlar.org/tr/unitree-egitim-cozumleri)
- [Tüm açık kaynak Unitree projeleri (Türkçe)](https://www.robotlar.org/tr/acik-kaynak)

## 🔗 Unitree Türkiye GitHub Ağı

**Modeller:** [Go2](https://github.com/Unitree-Turkiye/unitree-go2-turkce) · [Go2-W](https://github.com/Unitree-Turkiye/unitree-go2-w-turkce) · [B2](https://github.com/Unitree-Turkiye/unitree-b2-turkce) · [A2](https://github.com/Unitree-Turkiye/unitree-a2-turkce) · [G1](https://github.com/Unitree-Turkiye/unitree-g1-turkce) · [H2](https://github.com/Unitree-Turkiye/unitree-h2)
**Geliştirici:** [SDK2](https://github.com/Unitree-Turkiye/unitree-sdk2-turkce) · [XR Teleoperasyon](https://github.com/Unitree-Turkiye/unitree-teleoperasyon-turkce) · [LeRobot](https://github.com/Unitree-Turkiye/unitree-lerobot-turkce) · [RL Gym](https://github.com/Unitree-Turkiye/unitree-rl-gym-turkce) · [MuJoCo](https://github.com/Unitree-Turkiye/unitree-mujoco-turkce) · [ROS2](https://github.com/Unitree-Turkiye/unitree-ros2-turkce) · [VLA](https://github.com/Unitree-Turkiye/unitree-vla-turkce)
**G1 araştırma:** [G1 + Isaac GR00T](https://github.com/Unitree-Turkiye/unitree-g1-isaac-groot-turkce) · [G1 GEAR SONIC](https://github.com/Unitree-Turkiye/unitree-g1-gear-sonic-turkce) · [ProtoMotions](https://github.com/Unitree-Turkiye/nvidia-protomotions-turkce) · [G1 MotionBricks](https://github.com/Unitree-Turkiye/unitree-g1-motionbricks-turkce) · [G1 SOMA Retargeter](https://github.com/Unitree-Turkiye/unitree-g1-soma-retargeter-turkce) · [🏠 Unitree Türkiye](https://github.com/Unitree-Turkiye)

## 📚 Kaynaklar

Her proje için birincil kaynak (makale ya da proje sayfası) ve resmi depo:

| Proje | Makale / proje sayfası | Depo |
|---|---|---|
| BeyondMimic | [arXiv 2508.08241](https://arxiv.org/abs/2508.08241) · [beyondmimic.github.io](https://beyondmimic.github.io/) | [HybridRobotics/whole_body_tracking](https://github.com/HybridRobotics/whole_body_tracking) · [HybridRobotics/motion_tracking_controller](https://github.com/HybridRobotics/motion_tracking_controller) |
| GEAR SONIC | [arXiv 2511.07820](https://arxiv.org/abs/2511.07820) · [nvlabs.github.io/GEAR-SONIC](https://nvlabs.github.io/GEAR-SONIC/) | [NVlabs/GR00T-WholeBodyControl](https://github.com/NVlabs/GR00T-WholeBodyControl) · [nvidia/GEAR-SONIC](https://huggingface.co/nvidia/GEAR-SONIC) |
| ProtoMotions | [Belgeler](https://nvlabs.github.io/ProtoMotions/) | [NVlabs/ProtoMotions](https://github.com/NVlabs/ProtoMotions) |
| mjlab | [arXiv 2601.22074](https://arxiv.org/abs/2601.22074) | [mujocolab/mjlab](https://github.com/mujocolab/mjlab) |
| SuperMimic | [arXiv 2610.28480](https://arxiv.org/abs/2610.28480) · [supermimic.github.io](https://supermimic.github.io) | [HybridRobotics/supermimic](https://github.com/HybridRobotics/supermimic) |
| supermimic_vibed | README | [zita-ch/supermimic_vibed](https://github.com/zita-ch/supermimic_vibed) |
| InstantTrack | README | [Huy-Quang-Dao/InstantTrack](https://github.com/Huy-Quang-Dao/InstantTrack) |
| BlinkMimic | README | [fuyichen0725/BlinkMimic](https://github.com/fuyichen0725/BlinkMimic) |
| XMimic | [arXiv 2611.20452](https://arxiv.org/abs/2611.20452) | [nico-bohlinger/xmimic](https://github.com/nico-bohlinger/xmimic) |
| MacMimic | [air23zj.github.io/macmimic](https://air23zj.github.io/macmimic/) | [air23zj/macmimic](https://github.com/air23zj/macmimic) |
| LAFAN1 | Harvey ve ark., SIGGRAPH 2020 | [ubisoft/ubisoft-laforge-animation-dataset](https://github.com/ubisoft/ubisoft-laforge-animation-dataset) |
| AMASS | [Lisans](https://amass.is.tue.mpg.de/license.html) | [amass.is.tue.mpg.de](https://amass.is.tue.mpg.de/) |
| BONES-SEED | [Veri kartı](https://huggingface.co/datasets/bones-studio/seed) | [bones-studio/seed](https://huggingface.co/datasets/bones-studio/seed) |

Tablodaki tarih ve lisanslar 19 Kasım 2026'da kaynaklarından okundu. Depolar hızlı güncelleniyor, kullanmadan önce kaynağın güncel sürümüne bakın.

## 🤝 Katkı

Unitree G1 ya da başka bir insansı robot için yeni bir hareket izleme yöntemi yayımlandıysa tabloya eklemek için bir issue açın. Issue'da şu bilgileri verin:

- Proje adı, kurum ya da yazar, yayın tarihi
- Makale ya da proje sayfası ve resmi depo bağlantısı
- Kaynağın bildirdiği altyapı, donanım ve süre
- Gerçek robotta doğrulama durumu (kaynaktaki ifadeyle)
- Kod, model ve veri lisansı

Tabloya yalnız birincil kaynakta yazan olgular girer. Yazım düzeltmeleri için doğrudan pull request gönderebilirsiniz.

## 📄 Lisans

Bu deponun metni [CC-BY-4.0](LICENSE) lisanslıdır: Copyright (c) 2026 EP Robot Teknoloji (Robotlar.org). Metin özgün Türkçe bir araştırma özetidir, hiçbir kaynaktan kopyalanmadı. Anılan projelerin kodu, modelleri ve verileri kendi lisanslarına tabidir ve bu depoda bulunmaz.

---

> ℹ️ **Unitree Türkiye:** Unitree Robotics'in Türkiye resmi satış ve teknik distribütörü. Unitree G1 ailesi için teklif, kurulum, eğitim ve Türkçe geliştirici desteği: **[robotlar.org](https://www.robotlar.org/tr/rehber/unitree-turkiye-distributoru)** · 📧 info@robotlar.org

© 2026 **[www.robotlar.org](https://www.robotlar.org)**

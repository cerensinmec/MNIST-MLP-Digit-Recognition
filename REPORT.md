# Deney Değerlendirme Raporu — MNIST MLP

## 1. Amaç ve yöntem
MNIST el yazısı rakamlarının sınıflandırılması için PyTorch tabanlı, iki gizli katmanlı MLP modelleri eğitilmiştir. Görüntüler 28×28 pikselden 784 elemanlı vektöre düzleştirilmiştir. Gizli katmanlarda ReLU, çıkışta 10 sınıflı doğrusal katman kullanılmıştır. Eğitim CrossEntropyLoss ve Adam ile gerçekleştirilmiştir.

Veri ayrımı: 50.000 eğitim, 10.000 doğrulama, 10.000 test. Batch size 64; her deney 10 epoch.

## 2. Hiperparametre karşılaştırması
| Deney | Katmanlar | Öğrenme oranı | Değiştirilen özellik |
|---|---|---|---|
| 1 | 784–128–64–10 | 0.001 | Temel model |
| 2 | 784–256–128–10 | 0.001 | Gizli katman kapasitesi artırıldı |
| 3 | 784–256–128–10 | 0.0005 | Deney 2'ye göre öğrenme oranı azaltıldı |

Doğrulama kaybı ve doğruluğu epoch boyunca kaydedilmiş ve `results/` klasöründe grafiklerle karşılaştırılmıştır. En iyi ağırlıkların seçiminde doğrulama kaybı kullanılmıştır.

## 3. Seçilen modelin test sonuçları
Deney 2, en düşük doğrulama kaybı temelinde seçilmiştir. Kaydedilen en iyi ağırlıklar yüklenerek daha önce eğitimde kullanılmayan 10.000 test görüntüsü üzerinde ölçüm yapılmıştır.

- Test accuracy: **%97.52**
- Test loss: **0.0816**
- Doğru sınıflandırma: **9.752**
- Yanlış sınıflandırma: **248**

## 4. Kendi el yazısı deneyi
11 basamaklı el yazısı fotoğrafı gri tonlamaya çevrilmiş, uyarlamalı eşikleme ve kontur tespitiyle 11 ayrı rakam elde edilmiştir. Rakamlar oranları korunarak 28×28 piksellik siyah zeminlere yerleştirilmiş ve [0,1] aralığına normalize edilmiştir.

- Doğru sınıflandırılan rakam: **10/11**
- El yazısı doğruluğu: **%90.91**
- Hata: **10. rakamda `4` → `2`**, yanlış tahminin güveni **%71.39**.

MNIST test doğruluğu ile bu küçük el yazısı örnekleminin doğruluğu arasındaki fark **6.61 yüzde puandır**. Bunun nedeni kesin olarak belirlenememekle birlikte gerçek fotoğraftaki aydınlatma, kalem çizgisi, konum ve kişisel yazım biçimi farklılıkları etkili olabilir. Bu gözlem, veri dağılımı değişiminin (distribution shift) modelin genellemesine etkisini tartışmak için örnektir. Yalnızca 11 rakamdan oluşan örneklem, geniş kapsamlı bir genelleme iddiasını desteklemez.

## 5. Sınırlılıklar ve geliştirmeler
- MLP, piksellerin uzamsal komşuluk bilgisini doğrudan kullanmaz.
- Tek bir el yazısı fotoğrafı, farklı yazı stilleri ve aydınlatma koşullarını temsil etmez.
- Kontur tespiti gölge, birleşik karakter veya kopuk çizgilerden etkilenebilir.
- Daha geniş, bağımsız el yazısı örnekleriyle test; merkezleme ve çizgi kalınlığı ön işlemlerinin sistematik karşılaştırılması gelecekte değerlendirilebilir.

## 6. Sonuç
İki gizli ReLU katmanlı MLP, MNIST test kümesinde %97.52 doğruluğa ulaşmıştır. Kendi el yazısı fotoğrafındaki 11 rakamın 10'unu doğru tanımıştır. Tek yanlış tahmin, görüntü ön işlemenin ve veri dağılımı farklılıklarının önemini göstermektedir.

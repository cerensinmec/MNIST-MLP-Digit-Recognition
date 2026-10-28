# MNIST Veri Seti ile MLP Tabanlı El Yazısı Rakam Tanıma

## Proje amacı
Bu projede MNIST veri seti kullanılarak PyTorch ile çok katmanlı algılayıcı (MLP) modeli eğitilmiş, farklı hiperparametreler karşılaştırılmış ve model kendi el yazısıyla hazırlanmış rakamlar üzerinde test edilmiştir. Evrişimsel sinir ağı (CNN) veya önceden eğitilmiş model kullanılmamıştır.

## Veri seti ve ön işleme
- MNIST: 28×28 piksel gri tonlamalı, 0–9 rakam görüntüleri.
- Eğitim: 50.000; doğrulama: 10.000; test: 10.000 görüntü.
- Görüntüler `transforms.ToTensor()` ile [0,1] aralığına dönüştürülür.
- Eğitim/doğrulama ayrımı için sabit rastgele tohum (`42`) kullanılmıştır.
- Batch size: 64; epoch sayısı: 10; kayıp fonksiyonu: CrossEntropyLoss; optimizer: Adam.

## Modeller ve deneyler
| Deney | MLP mimarisi | Learning rate | Epoch |
|---|---|---|---|
| 1 | 784 → 128 → 64 → 10 | 0.001 | 10 |
| 2 | 784 → 256 → 128 → 10 | 0.001 | 10 |
| 3 | 784 → 256 → 128 → 10 | 0.0005 | 10 |

Her iki gizli katmanda ReLU aktivasyonu kullanılmıştır. Deney 2 ile Deney 3'te mimari aynı tutularak öğrenme oranının etkisi incelenmiştir.

## Sonuçlar
En düşük doğrulama kaybı ölçütüyle Deney 2'nin kaydedilmiş ağırlıkları seçilmiştir.

- **MNIST test doğruluğu:** %97.52 (9.752 / 10.000 doğru)
- **MNIST test kaybı:** 0.0816
- **Kendi el yazısı doğruluğu:** %90.91 (10 / 11 doğru)
- **Hatalı tahmin:** 10. rakamda gerçek `4`, tahmin `2` (model güveni %71.39)

Kendi el yazısı görüntüsü döndürülüp gri tonlamaya çevrilmiş, uyarlamalı eşikleme ve kontur tespitiyle rakamlar ayrılmış, her rakam en-boy oranı korunarak 28×28 boyutuna yerleştirilmiştir. Fotoğraftan gelen aydınlatma, çizgi kalınlığı ve yazım biçimi farklılıkları veri dağılımı değişimine (distribution shift) neden olabilir. 11 örneklik testin küçük olduğu unutulmamalıdır.

## Çalıştırma
1. `MNIST_MLP_Experiments.ipynb` dosyasını Google Colab'da açın.
2. Hücreleri sırayla çalıştırın. MNIST veri seti otomatik indirilir; deneylerin yeniden eğitilmesi zaman alır.
3. El yazısı bölümünde **kendi el yazınızın fotoğrafını** yükleyin. Notebook, fotoğrafı 90 derece saat yönünde döndürecek şekilde hazırlanmıştır; farklı yönlü fotoğraflarda bu adımı uyarlayın.
4. Model ağırlıkları `results/experiment2_best_model.pth` ve `results/experiment3_best_model.pth` içinde bulunur. Notebook'un önceki hücreleri çalıştırılmadan doğrudan tahmin hücreleri çalıştırılamaz.

## Dosyalar
- `MNIST_MLP_Experiments.ipynb`: deneylerin ve el yazısı tahminlerinin tüm kodları.
- `results/`: deney CSV'leri, grafikler, eğitilmiş model ağırlıkları ve el yazısı tahmin görseli.
- `handwriting/processed_digits/`: 11 adet 28×28 işlenmiş rakam görüntüsü.
- `handwriting/student_number_original.jpg`: orijinal el yazısı fotoğrafı.
- `REPORT.md`: deneylerin değerlendirilmesi.
- `requirements.txt`: Python bağımlılıkları.

## Notlar
Bu depo çalışmada kullanılan kodları ve elde edilen çıktıları içerir. Orijinal el yazısı fotoğrafı pakete eklenmiştir. Fotoğraftaki rakamlar modelin değerlendirilmesi için kullanılmıştır.

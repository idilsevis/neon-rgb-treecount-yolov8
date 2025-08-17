# neon-rgb-treecount-yolov8
Bu proje, "Weecology NEON Tree Evaluation" veri setini kullanarak YOLOv8 modeli ile havadan çekilmiş RGB görüntülerdeki ağaçları tespit etmeyi ve saymayı amaçlamaktadır. Çalışma, Google Colab ortamında, tek bir Jupyter Notebook hücresi içerisinde ortam kurulumundan model optimizasyonuna kadar tüm adımları içerecek şekilde tasarlanmıştır.

Proje Akışı
Notebook'taki adımlar şu şekildedir:

Ortam Kurulumu: Google Colab'da sıkça karşılaşılan numpy ve diğer kütüphane sürüm çakışmalarını gidermek için "HARD REPAIR" bölümü ile başlar.

Veri Hazırlığı:

XML formatındaki etiketler, YOLO formatına dönüştürülür.

Veri seti, eğitim (train) ve doğrulama (val) olarak ikiye ayrılır.

YOLOv8 eğitimi için gerekli olan data.yaml dosyası oluşturulur.

Model Eğitimi (2 Aşamalı):

Aşama 1: yolov8m.pt önceden eğitilmiş ağırlıkları kullanılarak model, 640x640 piksel çözünürlüğündeki görsellerle 100 epoch boyunca eğitilir.

Aşama 2 (Fine-tuning): İlk aşamada elde edilen en iyi model (best.pt), 1024x1024 piksel gibi daha yüksek çözünürlüklü görsellerle 20 epoch boyunca daha düşük bir öğrenme oranı (learning rate) ile ince ayardan geçirilir. Bu, modelin daha detaylı öğrenmesini sağlar.

Tahmin ve Optimizasyon:

Eğitilen modeller kullanılarak test görselleri üzerinde ağaç tespiti yapılır ve görsel önizlemeler oluşturulur.

Modelin sayım performansını en üst düzeye çıkarmak için en uygun güven (confidence) eşiği aranır.

Sonuçların Değerlendirilmesi ve Kaydedilmesi:

En iyi güven eşiği ile tüm görseller üzerinde nihai tahminler yapılır.

Tespit edilen ağaçların sayım sonuçları, MAE (Ortalama Mutlak Hata), MedAE (Medyan Mutlak Hata) ve Bias (Yanlılık) gibi metriklerle değerlendirilir.

Tüm çıktılar (tahmin etiketleri, sayım sonuçları, önizleme görselleri) Google Drive'a kaydedilir.

Kurulum
Proje, Google Colab üzerinde çalışacak şekilde optimize edilmiştir. Gerekli kütüphaneler notebook içerisinde otomatik olarak kurulur:

numpy==2.1.1

opencv-python-headless==4.10.0.84

torch, torchvision, torchaudio

ultralytics==8.3.179

rasterio

Kullanım
Veri Seti: "weecology-NeonTreeEvaluation" veri setini Google Drive'ınıza yükleyin.

Notebook Yapılandırması: Notebook'un başındaki CONFIG bölümünde bulunan BASE_PATH değişkenini, veri setinin bulunduğu Google Drive yolu ile güncelleyin. Diğer tüm yollar bu temel yola göre otomatik olarak ayarlanacaktır.

Çalıştırma: Tüm hücreleri sırasıyla çalıştırın. Notebook, kurulum, veri hazırlığı, eğitim, tahmin ve değerlendirme adımlarını otomatik olarak gerçekleştirecektir.

Sonuçlar ve Değerlendirme
Eğitim Sonuçları:

640x640 Eğitimi:

Epoch: 100

En İyi mAP50-95: 0.217

1024x1024 Fine-tuning:

Epoch: 20

En İyi mAP50-95: 0.213

Sayım Performansı (1024x1024 Modeli için):

Modelin pratik kullanımda en iyi performansı göstermesi için farklı güven eşikleri taranmış ve en iyi sonuçlar aşağıdaki gibi elde edilmiştir:

Önerilen Güven Eşiği (Confidence Threshold): 0.20


Ortalama Mutlak Hata (MAE): 6.736 (Görüntü başına ortalama hata)

Medyan Mutlak Hata (MedAE): 3.000 (Görüntülerin yarısındaki hata 3 veya daha az)

Yanlılık (Bias): -3.90 (Model, gerçek ağaç sayısından daha az sayma eğilimindedir)

Bu sonuçlar, modelin ağaç sayım görevinde makul bir başarı gösterdiğini, ancak sistematik olarak eksik sayım yapma eğiliminde olduğunu ortaya koymaktadır.

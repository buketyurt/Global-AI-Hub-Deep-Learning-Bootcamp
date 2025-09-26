# PlantVillage Bitki Hastalığı Sınıflandırma Projesi

## 1) Proje Özeti ve Hedef
Bu projede **PlantVillage görüntüleri** üzerinde, sıfırdan tasarlanmış bir **Custom CNN** ile bitki hastalığı sınıflandırması yapılmaktadır.  
Projede aşağıdaki adımlar uygulanmıştır:
- Veri ön işleme
- Veri artırma (augmentation)
- Eğitim stratejileri (early stopping, LR azaltma, model checkpoint)
- Değerlendirme metrikleri (accuracy, macro/weighted F1)
- TTA (test-time augmentation)
- Eigen-CAM görselleştirmeleri
- Çıktıların diske kaydı

---

## 2) Veri Seti: PlantVillage (PlantDisease)
Bu çalışmada kullanılan veri kümesi, Kaggle üzerinde yayımlanan **PlantVillage (PlantDisease)** veri setidir. Veri seti, farklı bitki türlerinin yaprak görüntülerini içermekte olup hem sağlıklı hem de çeşitli hastalıklara sahip örneklerden oluşmaktadır.

- **Toplam Görüntü Sayısı:** ~87.000  
- **Sınıf Sayısı:** 38 (bitki türü + hastalık kombinasyonu)  
- **Veri Tipi:** Renkli yaprak görüntüleri (RGB)  
- **Dosya Yapısı:** Her hastalık/tür kombinasyonu için ayrı klasör  
  *(Örn: Tomato___Late_blight, Potato___Early_blight, Pepper___healthy)*

Görseller, laboratuvar ve kontrollü ortam koşullarında çekildiği için yüksek çözünürlüklü ve düzenli etiketlenmiştir.

---

## 3) Kullanılan Yöntemler
Bu çalışmada bitki hastalıklarının sınıflandırılması için derin öğrenme tabanlı yöntemler kullanılmıştır.

- **Veri Artırma (Data Augmentation):** Döndürme, yakınlaştırma, yansıtma, parlaklık değişimleri.  
- **Erken Durdurma ve Model Kaydedici:** Overfitting’i azaltmak için doğrulama kaybı takip edilmiştir.  
- **Hiperparametre Optimizasyonu:** Öğrenme oranı, batch size, epoch sayısı test edilerek en iyi kombinasyon seçilmiştir.

Bu yöntemler birlikte kullanılarak modelin doğruluk ve genelleme başarısı artırılmıştır.

---

## 4) Model Sonuçlarının Özeti
Eğitim süreci boyunca model, başlangıçta düşük doğruluk oranından (Epoch 1’de %51 eğitim, %48 doğrulama) hızlı bir şekilde yükselmiş ve 10. epoch civarında %94 seviyelerine ulaşmıştır.

- **Genel Doğruluk (Accuracy):** %93.8  
- **Macro F1:** %93.5  
- **Weighted F1:** %93.8  

### Sınıf Bazlı Performans
- Bazı sınıflar: %100 doğruluk (örn. Potato healthy, Tomato Leaf Mold)  
- Zor sınıflar: Tomato healthy → %83.6 doğruluk  
- **Plant-wise doğruluk:**  
  - Patates: %98.1  
  - Biber: %96.4  
  - Domates: %92.9  
  - Genel Ortalama: %95.8

Confusion matrix doğru sınıflandırmaların baskın olduğunu, özellikle domates sağlıklı/hasta ayrımında hata payı olduğunu göstermektedir.

---

## 5) Overfitting / Underfitting Analizi
- **Epoch 1–5:** Eğitim ve doğrulama eğrileri birlikte yükselmiş, model doğru öğrenmiştir.  
- **Epoch 6–7:** Eğitim doğruluğu artarken doğrulama düşmüş → **Overfitting belirtisi**.  
- **Epoch 8–14:** LR azaltımı ile doğrulama toparlanmış (%93). Early stopping için ideal dönem.  
- **Epoch 15 sonrası:** Hafif overfitting eğilimi olsa da kontrol altındadır.

---

## 6) Görselleştirme Sonuçları
- **Doğruluk/Kayıp Grafikleri:** Eğitim eğrisi düzenli, doğrulama eğrisi dalgalı → küçük overfitting işaretleri.  
- **Eigen-CAM:** Model yapraklardaki hastalık bölgelerine odaklanmıştır.  
- **Confusion Matrix:** Genel olarak doğru sınıflandırmalar yoğun, ancak domates sınıfında belirgin hata vardır.

---

## 7) Genel Değerlendirme
- Model **%93–95 doğruluk** ile başarılıdır.  
- Hafif overfitting belirtileri görülmüştür ancak LR azaltma ve dropout ile kontrol edilmiştir.  
- Özellikle domates sınıfında veri artırma ve dengesizlik giderme ile geliştirme yapılabilir.  
- Genel olarak model, **bitki hastalıklarını yüksek doğrulukla sınıflandırabilecek** seviyededir.

---

## 8) Kaynak
[Kaggle Notebook](https://www.kaggle.com/code/buketyurt/global-ai-hub-deep-learning-bootcamp2)
]

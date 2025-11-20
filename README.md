# EMBER-Hybrid-Detection-SAE-TopK-LGBM
EMBER 2024 veri seti üzerinde geliştirilen SAE–Top-K–LightGBM hibrit mimarisi ile malware tespiti için kullanılan tüm kodlar, deneysel analizler, grafikler ve LaTeX dosyaları.

---

## 📌 1. Çalışmanın Amacı
Bu çalışma, EMBER 2024 veri seti üzerinde gerçekleştirilen kapsamlı bir Ar-Ge (R&D) projesinin kod, analiz ve deneysel sonuçlarını içermektedir. Çalışmanın temel amacı, Windows PE dosyaları için gerçek zamanlı çalışabilen, düşük gecikmeli, hafif ve yüksek doğruluk oranına sahip bir zararlı yazılım tespit modeli geliştirmektir. Bu doğrultuda, SAE tabanlı derin temsiller, Top-K özellik seçimi ve LightGBM sınıflandırıcısından oluşan hibrit mimari tasarlanmış; model hem 5-Fold çapraz doğrulama hem de zaman temelli (temporal) test senaryoları ile kapsamlı biçimde değerlendirilmiştir.

Tüm deneyler iki ana senaryoda gerçekleştirilmiştir:

- **5-Fold Stratified Cross-Validation**
- **Temporal (Zaman Bazlı) Değerlendirme**

---

## 📌 2. Klasör Yapısı
```
code/
├── 5Fold/
│   ├── DL_OFF_5Fold.py
│   ├── DL_PL5DA_5Fold.py
│   ├── DL_VAE_Robust_5Fold.py
│   ├── Hybrid_OFF_5Fold.py
│   ├── Hybrid_OFF_LGBM_5Fold.py
│   ├── Hybrid_OFF_SAE_LGBM_5Fold.py
│   ├── Hybrid_OFF_TopK_LGBM_5Fold.py
│   ├── Hybrid_PL5DA_5Fold.py
│   ├── Hybrid_VAE_5Fold.py
│   ├── ML_OFF_5Fold.py
│   ├── ML_PL5DA_5Fold.py
│   └── ML_VAE_5Fold.py
│
├── Temporal/
│   ├── DL_Temporal.py
│   ├── Hybrid_Temporal.py
│   └── ML_Temporal.py
│
└── README.md
```
---

## 📌 3. Kullanılan Veri Seti

Bu çalışmada kullanılan veri seti **EMBER 2024** olup tamamen kamuya açıktır.

🔗 Veri seti bağlantısı:  
[https://github.com/elastic/ember](https://github.com/FutureComputing4AI/EMBER2024)

400.397 örnekten oluşan **dengeli alt küme**, araştırmacı tarafından oluşturulmuştur ve telif nedeniyle GitHub’da paylaşılmamaktadır.  
Ancak tüm veri hazırlama adımları çalışmanın makale kısmında ayrıntılı biçimde açıklanmıştır.

---

## 📌 4. Modeller

### 🔹 Makine Öğrenmesi (ML)
- CatBoost  
- Extra Trees  
- Gradient Boosting  
- HistGradientBoosting  
- SGDClassifier  

### 🔹 Derin Öğrenme (DL)
- Wide&Deep
- DNN  
- MLP_Mixer 
- ResnetMLP 
- gMLP-Tabular

### 🔹 Boyut İndirgeme Yöntemleri
- PLS-DA (Partial Least Squares Discriminant Analysis)
- VAE (Variational Autoencoder) 

### 🔹 Önerilen Hibrit Model
- **SAE (Autoencoder) – 512-256-256-Latent 256**
- **Top-K Feature Selection (LightGBM Importance)**
- **LightGBM Sınıflandırıcı**

---

## 📌 5. Sonuçların Özeti

### ✔ 5-Fold CV Sonuçları (Hibrit)
- **Accuracy:** %98.30  
- **Recall:** %98.07  
- **Precision:** %98.53  
- **Specificity:** %98.54  
- **AUC-ROC:** %99.83  

### ✔ Temporal Test Sonuçları (Hibrit)
-  **Accuracy** 0.9670 
-  **Recall**   0.9610 
-  **Specificity**  0.9730 
-  **Precision**  0.9726 
-  **F1-Score**  0.9688 
-  **F1-Weighted** 0.9670
-  **AUC-ROC**  0.9933 

- **Latency:** 0.008 ms/örnek  
- **Throughput:** 120.180 örnek/s  
- **Model Boyutu:** 21.30 MB  
- **Eğitilebilir Parametre:** **3.03M**

---

## 📌 6. Çalıştırma

Her Python dosyası bağımsız olarak terminal üzerinden çalıştırılabilir.

### Örnek kullanım:

python Hybrid_OFF_SAE_LGBM_5Fold.py

⚠ Not: Model Adı Hakkında Açıklama

Bu projedeki bazı Excel sonuç dosyalarında "Model" kolonu otomatik olarak Hybrid_SAE_LGBM şeklinde görünmektedir.
Bu durum bazı senaryolarda yalnızca çıktı dosyasındaki etiketleme hatasından kaynaklanmakta olup, modelin gerçek yapısını etkilememektedir.

✔ OFF (5Fold) İçin Model Adı Doğrudur

OFF veri temsili (5Fold) için kullanılan nihai hibrit model gerçekten:
➡ Hybrid_SAE_LGBM
Dolayısıyla OFF sonuçlarında görülen model adı doğru ve geçerlidir.

✔ Ablasyon (Bileşen Analizi) Modelleri Hakkında

Bu çalışmada hibrit mimarinin bileşen katkılarını değerlendirmek amacıyla üç ayrı ablation modeli kullanılmıştır:

Hybrid_SAE_LGBM

Hybrid_TopK_LGBM

Hybrid_LGBM

Bazı ara çıktı dosyalarında bu üç model de Hybrid_SAE_LGBM olarak görünebilmektedir.
Bu, yalnızca etiketleme kaynaklı bir isimlendirme hatasıdır; performans metrikleri doğru model üzerinden hesaplanmıştır.

✔ Doğru Etiketlenmiş Excel Dosyaları Mevcuttur

Her bir ablation modeli için doğru şekilde etiketlenmiş Excel çıktı dosyaları ayrıca eklenmiştir.
Bu dosyalarda model adları doğru belirtilmiş olup, analiz ve karşılaştırmalar için güvenle kullanılabilir.

📧 İletişim
Sultan Tazefidan
📩 sultantazefidan.1@gmail.com


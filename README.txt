# 🛒 Fraud Detection
### Multi-Entity Velocity Modeling & Deep Training Strategy

Bu proje, ödeme verileri üzerinde, dengesiz veri setlerinde (imbalanced datasets) dolandırıcılık tespitini optimize etmek amacıyla geliştirilmiştir. Toplam **3.12 Milyon işlem** içinde sadece **%0.315** (Binde 3) oranında bulunan fraud vakalarını, operasyonel verimliliği maksimize edecek şekilde tespit eder.

---

## 🏆 Başarı Metrikleri (Şampiyon Model)

Modelin başarısı, klasik doğruluk (accuracy) yerine, operasyonel iş değerine odaklanan **Top-1% Recall** metriği ile ölçülmüştür.

| Metrik | Değer | İş Anlamı |
| :--- | :--- | :--- |
| **Top-1% Recall** | **%55.8** | Operasyon ekibi işlemlerin **sadece %1'ini** inceleyerek, tüm dolandırıcılıkların **%55.8'ini** yakalayabilir. |
| **ROC-AUC** | **0.981** | Modelin suçlu ile masumu genel ayırma başarısı. |
| **Verimlilik Artışı** | **56 Kat** | Rastgele incelemeye kıyasla 56 kat daha verimli operasyon. |

---

## 🚀 Proje Mimarisi ve Stratejik Yaklaşım

Standart bir sınıflandırma probleminden farklı olarak, bu projede **3 katmanlı bir optimisazyon stratejisi** uygulanmıştır:

### 1. Leakage-Free Feature Engineering (Sızıntısız Özellik Mühendisliği)
Geleceği görme (data leakage) hatasını önlemek için tüm hesaplamalarda **`closed='left'`** pencereleme yöntemi kullanılmıştır. Model, işlem anındaki veriyi görmez, sadece o andan önceki tarihçeyi analiz eder.

### 2. Multi-Entity Velocity (Çoklu Varlık Hızı)
Dolandırıcılar kartı değiştirse bile davranış izlerini bırakır. Bu nedenle sadece Kart ID değil, üç farklı boyutta hız profili çıkarılmıştır:
* **Card Velocity:** Kartın son 1 saat/24 saatteki hareketliliği.
* **User Velocity (GSM):** Kart değişse bile, aynı telefon numarasından yapılan işlem sıklığı.
* **Merchant Velocity:** İş yerine yapılan ani yüklenme saldırıları (Attack Vectors).

### 3. Deep Training Stratejisi (No Early Stopping)
Standart modellemede `Early Stopping` kullanıldığında modelin %51.6 başarıda tıkandığı görülmüştür.
* **Müdahale:** Erken durdurma devre dışı bırakılmış ve modelin **1200 iterasyon** boyunca "zor ve karmaşık" fraud desenlerini öğrenmesine izin verilmiştir.
* **Sonuç:** Bu strateji performansı **%51.6'dan %55.8'e** taşımıştır.

---

## 📈 Etki Analizi (Ablation Study)

Yapılan mühendislik çalışmalarının modele net katkısı sayısal olarak kanıtlanmıştır:

* **Baseline (Ham Veri):** %44.7 Recall (Temel kurallar).
* **+ Velocity Features:** %51.6 Recall (Davranışsal analiz eklendiğinde).
* **+ Deep Training (Final):** **%55.8 Recall** (Öğrenme kısıtları kaldırıldığında).

---

## 🛠 Validasyon Stratejisi: Time-Based Quantile Split

Fraud dinamik bir yapıdadır. Rastgele (Random) ayrım yerine, gerçek hayat senaryosunu simüle eden **Zaman Bazlı Ayrım** kullanılmıştır:

* **Train (%70):** Temmuz - Ağustos (Geçmiş).
* **Validation (%15):** Eylül Başı (Optimizasyon).
* **Test (%15):** Eylül Sonu (Gelecek - Hiç görülmemiş veri).

---

## 📂 Dosya ve Notebook Yapısı

* **`00_EDA.ipynb`**: Veriyi anlama, eksik veri analizi, zaman dağılımı ve test setindeki fraud azlığının (Dataset Shift) tespiti.
* **`01_Feature_Engineering.ipynb`**: Ham veriden sızıntısız (leakage-free) hız, oran ve zaman farkı değişkenlerinin üretilmesi.
* **`02_Model_Selection_Ablation.ipynb`**: Farklı özellik setlerinin (Ham vs Velocity) modele katkısının izole testlerle ölçülmesi.
* **`03_Train_Model.ipynb`**: **Deep Training** stratejisi ile final CatBoost modelinin eğitilmesi. Model ve artifactlerin (JSON) kaydedilmesi.
* **`04_Inference_Demo.ipynb`**: Canlı ortam simülasyonu. `Schema Enforcement` ile güvenli tahmin üretimi.
* **`05_Feature_Importance.ipynb`**: Modelin yorumlanabilirliği. SHAP analizi ile karar mekanizmasının doğrulanması.

---

## ⚙️ Kurulum ve Çalıştırma

1.  **Gereksinimleri Yükleyin:**
    ```bash
    pip install -r requirements.txt
    ```
2.  **Veri Seti:** `iyzico_fraud_data.csv` dosyasını `data/raw/` klasörüne ekleyin.
3.  **Pipeline'ı Çalıştırın:** Notebookları sırasıyla (00 -> 05) çalıştırın.

---

## 💻 Kullanılan Teknolojiler

* **Dil:** Python 3.10+
* **Model:** CatBoost Classifier
* **Analiz:** Pandas, NumPy
* **Görselleştirme:** Matplotlib, Seaborn
* **Açıklanabilirlik:** SHAP


iyzico Fraud Detection Case Study

Proje Özeti ve Başarı Metrikleri
ROC-AUC: 0.984 Anlamı: Modelin genel sıralama başarısı.
Top-1% Recall: %54.5 Anlamı: Operasyon ekibi işlemlerin sadece yüzde 1'ini inceleyerek, tüm dolandırıcılıkların yarısından fazlasını yakalayabilir.
Valid PR-AUC: 0.274 Anlamı: Dengesiz veri setinde modelin başarısı.

Proje Mimarisi ve Yaklaşım
Bu projeyi standart bir sınıflandırma probleminden ayıran 3 temel strateji uygulanmıştır:
Leakage-Free Feature Engineering (Sızıntısız Özellik Mühendisliği) Geleneksel yöntemlerde yapılan geleceği görme hatası, closed='left' parametresi ile engellenmiştir. Değişkenler hesaplanırken işlem anındaki veri değil, sadece geçmiş veri kullanılmıştır.
Multi-Entity Velocity (Çoklu Varlık Hızı) Dolandırıcılar kartı değiştirse bile davranış izlerini bırakır. Bu yüzden sadece Kart ID değil, üç farklı boyutta hız analizi yapılmıştır:
Card Velocity: Kartın son 1 saat ve 24 saatteki hareketliliği.

User Velocity (GSM): Aynı telefon numarasından yapılan işlem sıklığı.

Merchant Velocity: İş yerine yapılan ani yüklenmeler.

Ablation Study (Etki Analizi) Hangi özellik grubunun modele ne kadar katkı sağladığı test edilmiştir. Sadece kart verisi kullanıldığında düşük olan yakalama oranı, GSM ve Merchant verileri eklendiğinde ciddi oranda artmıştır.

Dosya ve Notebook Yapısı
00_EDA.ipynb: Veriyi anlama, eksik veri analizi, zaman dağılımı ve test setindeki fraud azlığının tespiti.

01_Feature_Engineering.ipynb: Ham veriden hız, oran ve zaman farkı değişkenlerinin sızıntısız olarak üretilmesi.

02_Model_Selection_Ablation.ipynb: Hangi değişken setlerinin modele katkı sağladığının analizi.

03_Train_Model.ipynb: Final CatBoost modelinin eğitilmesi. Model dosyasının ve eğitim parametrelerinin kaydedilmesi.

04_Inference_Demo.ipynb: Canlı ortam simülasyonu. Modelin yüklenip yeni gelen veriye tahmin üretmesi.

05_Feature_Importance.ipynb: Modelin yorumlanabilirliği. Değişken önem düzeyleri ve SHAP analizi.

Kurulum ve Çalıştırma
Gerekli kütüphaneleri yükleyin: pip install -r requirements.txt

Veri setini data/raw/ klasörüne ekleyin.

Notebookları sırasıyla çalıştırın (00 -> 05).

Kullanılan Teknolojiler
Dil: Python 3.10+ Model: CatBoost Analiz: Pandas, NumPy, Matplotlib, Seaborn Açıklanabilirlik: SHAP
---

## 🏆 Yarışma Başarı Sonuçları

Bu çalışma, ING Hubs Datathon sürecinde değerlendirilmiş ve aşağıdaki sıralama sonuçlarını elde etmiştir:

- **Public Leaderboard:** İlk %3’lük dilimde yer aldı
- **Private Leaderboard:** Son değerlendirmede **ilk %10’luk dilim** içinde konumlandı

---

# ING Hubs Datathon - Churn Prediction & Stacking Ensemble Model

Bu proje, ING Hubs Datathon kapsamında müşterilerin bankadan ayrılma (churn) olasılığını tahmin etmek için geliştirilmiştir.  
Veri üzerinde çeşitli feature engineering işlemleri uygulanmış ve ardından makine öğrenimi modelleri ile tahminleme yapılmıştır.

---

## Veri Seti

Çalışmada kullanılan veri, ön işleme ve özellik zenginleştirme (feature engineering) işlemlerinden geçirilerek son haline getirilmiştir.  
Bu süreçte:

- Kategorik değişkenler modele uygun formata dönüştürüldü
- Eksik ve tutarsız veriler temizlendi
- Aykırı değerlerin etkisi azaltıldı
- Alan bilgisine dayalı yeni özellikler eklendi
- Model performansını artırmak için ek türev değişkenler oluşturuldu

Bu işlemler sonucunda eğitim ve test için **final veri setleri** elde edilmiştir.

---

## Özel Metrik Kullanımı

Klasik ROC AUC metriğinin yanında iş etkisini anlamak adına:

- **Gini Katsayısı**
- **Recall @ Top %10**
- **Lift @ Top %10**

metrikleri de kullanılmıştır.

Bu metriklerin her biri baseline değere göre normalize edilerek aşağıdaki bileşik skor elde edilmiştir:
Final Skor = 0.4 * Gini + 0.3 * Recall@10% + 0.3 * Lift@10%

---

## Modelleme Yaklaşımı

Projede iki temel model uygulanmıştır:

- **CatBoostClassifier**  
  Kategorik değişkenleri doğal olarak desteklediği için tercih edildi. Ek encoding ihtiyacı yoktur.

- **LightGBM (Gradient Boosting Decision Tree Yapısı)**  
  Hızlı ve güçlü performans verdiği için kullanıldı.

Her iki model de **Stratified 5-Fold Cross Validation** ile eğitilmiştir.

Base model tahminleri elde edildikten sonra bu sonuçlar bir araya getirilerek:

- **Logistic Regression** meta modeli ile **Stacking Ensemble** yöntemi uygulanmıştır.

---

## Performans Sonuçları

- CatBoost ortalama AUC: ~0.720
- LightGBM ortalama AUC: ~0.719
- **Stacking meta model AUC: 0.7217**

Stacking yaklaşımı, tek başına modellerden daha dengeli ve güçlü bir performans sağlamıştır.

---

## Model Akış Özeti

1. Veri yüklenir ve feature engineering uygulanmış final veri seti kullanılır.
2. CatBoost ve LightGBM modelleri 5-fold CV ile eğitilir.
3. Her fold için OOF tahminler kayıt edilir.
4. Bu tahminler meta modelin giriş değişkenleri olarak kullanılır.
5. Logistic Regression meta model final tahmini üretir.

---


# ING Hubs Datathon — Churn Prediction & Stacking Ensemble Model

Bu proje, ING Hubs Datathon kapsamında müşterilerin bankadan ayrılma (churn) olasılığını tahmin etmek amacıyla geliştirilmiştir.

**Yarışma Başarısı:**
- Public leaderboard sonuçlarında ilk **%3** içinde tamamlandı.
- Private leaderboard final değerlendirmesinde ilk **%10** içinde yer aldı.



---

## 🎯 Projenin Amacı

Bu çalışmanın temel hedefi, churn riski taşıyan müşterileri tespit ederek:
- Müşteri kaybını azaltmak,
- Müşteri sadakat stratejilerini güçlendirmek,
- Hedefli kampanya ve iletişim süreçlerini optimize etmek

için kullanılabilir bir tahmin modeli geliştirmektir.

---

## 📂 Veri Hazırlama ve Feature Engineering

Ham veri doğrudan modele verilmemiş, performansı artırmak amacıyla çeşitli veri işleme adımları uygulanmıştır:

- Kategorik değişkenler modele uygun formata dönüştürüldü.
- Eksik ve hatalı veriler temizlendi.
- Aykırı değerlerin etkisi azaltıldı.
- Alan bilgisine dayalı yeni açıklayıcı değişkenler oluşturuldu.
- Model performansını güçlendirmek için ek türev özellikler eklendi.

Bu işlemler sonucunda **train_data_v7** ve **test_data_v7** adlı final veri setleri elde edilmiştir.

---

## 🧠 Modelleme Yaklaşımı

Modelleme sürecinde iki farklı makine öğrenimi algoritması kullanılmıştır:

- **CatBoostClassifier:**  
  Kategorik değişkenleri doğal olarak işlediği için ek encoding ihtiyacı olmadan etkili sonuç verdi.

- **LightGBM (Gradient Boosting Decision Trees):**  
  Hızlı eğitim ve güçlü ayrıştırma kapasitesi sayesinde modelin performansını destekledi.

Her iki model, sonuçların sağlam ve dengeli olmasını sağlamak için **Stratified 5-Fold Cross Validation** yöntemiyle eğitildi.

Bu modellerden elde edilen tahminler daha sonra birleştirilerek:
- **Logistic Regression** meta modeli ile **Stacking Ensemble** yöntemi uygulandı.

Bu sayede iki modelin güçlü yönleri tek bir nihai modelde toplandı.

---

## 📈 Model Performansı

Eğitim sürecinde gözlemlenen ortalama sonuçlar:

- CatBoost modeli yaklaşık **0.720 AUC**
- LightGBM modeli yaklaşık **0.719 AUC**
- Stacking meta modeli **0.7217 AUC**

Stacking yaklaşımı, tekil modellere göre daha kararlı ve güçlü bir tahmin performansı sağlamıştır.

---

## ⚙️ Model İş Akışı Özeti

1. Feature engineering uygulanmış final veri setleri yüklenir.
2. CatBoost ve LightGBM modelleri 5-fold CV ile ayrı ayrı eğitilir.
3. Her fold için out-of-fold tahminler kaydedilir.
4. Bu tahminler meta modele giriş olarak kullanılır.
5. Logistic Regression meta modeli nihai tahminleri üretir.

---


# 🎓 Student Performance Prediction & Early Warning System

![Python](https://img.shields.io/badge/Python-3.14-blue.svg)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange.svg)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen.svg)

Bu proje, öğrencilerin dönem sonu akademik başarılarını (final notu: `G3`) tahmin etmek ve henüz sınavlar başlamadan önce akademik risk altındaki öğrencileri tespit etmek amacıyla geliştirilmiş uçtan uca bir **Veri Analitiği ve Makine Öğrenmesi** çalışmasıdır.

Proje sürecinde ham verinin analizi, **Özellik Mühendisliği (Feature Engineering)**, **Veri Sızıntısı (Data Leakage)** yönetimi ve **Açıklanabilir Yapay Zeka (XAI)** yöntemleri uygulanmıştır.

---

## 📌 İş Problemi ve Yaklaşım (Business Problem)

Geleneksel eğitim yönetim sistemleri, öğrencilerin başarısızlığını ancak ara sınavlar (`G1`, `G2`) açıklandıktan sonra (reaktif) tespit edebilir. Bu durum, müdahale etmek için geç kalınmasına neden olur.

Bu projenin temel amacı:

1. **Reaktif Not Tahmini:** Ara sınav notları dahil edildiğinde modelin tahmin başarısını ölçmek.
2. **Proaktif Erken Uyarı Sistemi (Early Warning System):** Sınav notları (`G1`/`G2`) veri setinden çıkarılarak, dönemin ilk gününde sadece davranışsal ve demografik verilerle riski tespit eden proaktif bir model kurgulamak.

---

## 📊 Veri Seti (Dataset)

Çalışmada UCI Machine Learning Repository üzerinde yer alan **Student Performance Dataset** kullanılmıştır.

- **Gözlem Sayısı:** 395 Öğrenci
- **Hedef Değişken (`y`):** `G3` - Dönem Sonu Final Notu (0 - 20 puan arası)
- **Özellikler (`X`):** Yaş, devamsızlık, haftalık çalışma süresi, aile eğitim düzeyi, sosyal aktiviteler, geçmiş ders başarısızlıkları vb.

---

## 🛠️ Özellik Mühendisliği (Feature Engineering)

İş mantığı (Business Logic) doğrultusunda veriden yeni ve anlamlı değişkenler türetilmiştir:

- **`is_high_risk` (Risk Bayrağı):** Devamsızlığı 20 günden fazla VEYA geçmiş başarısızlığı (`failures`) olan öğrenciler riskli (`1`) olarak etiketlenmiştir.
- **`total_parent_edu`:** Anne (`Medu`) ve baba (`Fedu`) eğitim seviyeleri birleştirilerek aile içi toplam akademik destek metriği oluşturulmuştur.
- **Categorical Encoding:** Metin veri tipleri `pd.get_dummies(drop_first=True)` yöntemi ile sayısallaştırılmıştır.

---

## 🚀 Model Performansı ve Kıyaslama

Model eğitimi %80 Eğitim (Train) ve %20 Test ayrımıyla yapılmıştır. İki farklı yaklaşım test edilmiştir:

### 1. Senaryo: Ara Sınav Notları Dahil (G1 ve G2 Var)

- **Random Forest Regressor:** $R^2 = 0.82$ | $\text{MAE} = 1.18\text{ puan}$
- **Lineer Regresyon:** $R^2 = 0.73$ | $\text{MAE} = 1.63\text{ puan}$

> **Bulgu:** Model kararlarının %80'ini `G2` (2. dönem notu) oluşturmaktadır. Bu durum reaktif tahmin için yüksek performans sağlasa da erken uyarı amacı için "Veri Sızıntısı (Data Leakage)" yaratmaktadır.

### 2. Senaryo: Proaktif Erken Uyarı Modeli (G1 ve G2 Yok)

Sınavlar yapılmadan önceki durumu modellemek amacıyla ara notlar çıkarılmıştır.

- **Random Forest (Erken Uyarı):** Dönem başında çalışabilen bu modelde karar mekanizmasını yönlendiren en kritik faktörler:
  1. **Devamsızlık (`absences`):** ~%19 önem derecesi
  2. **Geçmiş Başarısızlıklar (`failures`):** ~%11 önem derecesi
  3. **Sağlık Durumu (`health`) & Sosyal Yaşam (`goout`)**
  4. **Türetilen Risk Bayrağı (`is_high_risk`) & Aile Eğitimi (`total_parent_edu`)**

---

## 📈 Öne Çıkan Görseller ve Analizler

- **Aykırı Değer Tespiti:** Devamsızlık analizinde 20 günü aşan öğrencilerin yüksek başarı (15+ not) elde etme ihtimalinin sıfıra yaklaştığı tespit edilmiştir.
- **Feature Importance:** Random Forest modelinin karar verirken odaklandığı değişkenler bar grafikleri ile doğrulanmıştır.

---

## 💻 Kullanılan Teknolojiler

- **Dil:** Python 3.14
- **Veri İşleme:** Pandas, NumPy
- **Görselleştirme:** Matplotlib, Seaborn
- **Makine Öğrenmesi:** Scikit-Learn (`train_test_split`, `LinearRegression`, `RandomForestRegressor`)
- **Geliştirme Ortamı:** VS Code, Jupyter Notebook (`.ipynb`)

---

## ⚙️ Kurulum ve Çalıştırma

Projeyi yerel makinenizde çalıştırmak için:

```bash
# 1. Depoyu klonlayın
git clone [https://github.com/kullanici_adiniz/ogrenci_not_tahmini.git](https://github.com/kullanici_adiniz/ogrenci_not_tahmini.git)

# 2. Proje dizinine geçin
cd ogrenci_not_tahmini

# 3. Gerekli kütüphaneleri yükleyin
pip install pandas numpy scikit-learn matplotlib seaborn notebook

# 4. Jupyter Notebook'u başlatın
jupyter notebook proje.ipynb
```

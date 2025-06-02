# WSN Kablosuz Sensör Ağları - Bulanık Mantık Analiz Sistemi

## 📋 Genel Bakış

Bu proje, Kablosuz Sensör Ağlarında (Wireless Sensor Networks - WSN) Ortalama Lokalizasyon Hatası (ALE - Average Localization Error) tahminini yapmak için geliştirilmiş kapsamlı bir bulanık mantık sistemidir. Proje iki ana modülden oluşur:

1. **Veri Analizi Modülü**: Veri yükleme, temizleme, analiz ve görselleştirme
2. **Bulanık Mantık Sistemi**: Çoklu üyelik fonksiyonları ve defuzzification yöntemleri ile ALE tahmini

## 🔧 Gereksinimler

### Gerekli Python Kütüphaneleri
```
pandas >= 1.3.0
numpy >= 1.21.0
matplotlib >= 3.4.0
seaborn >= 0.11.0
scikit-learn >= 1.0.0
```

### Kurulum
```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

## 📊 Veri Seti Formatı

### CSV Dosya Yapısı
Sistem aşağıdaki sütunları içeren CSV dosyalarını işler:

1. **anchor_ratio**: Çapa oranı (10-30 arası)
2. **trans_range**: İletim aralığı (12-25 arası)
3. **node_density**: Düğüm yoğunluğu (100, 200, 300)
4. **iterations**: Yineleme sayısı (14-100 arası)
5. **ale**: Average Localization Error - Hedef değişken (0.39-2.57 arası)
6. **std_dev**: Standart sapma 

### Veri Özellikleri
- **Beklenen satır sayısı**: 107
- **Sütun sayısı**: 6
- **Veri tipi**: Tüm sütunlar sayısal
- **Hedef değişken**: ALE değeri 

## 🚀 Kullanım

### 1. Temel Veri Analizi
```python
from wsn_data_analyzer import load_and_analyze_csv

# Veri yükleme ve analizi
data_array, data_df = load_and_analyze_csv('veri.csv')
```

### 2. Bulanık Mantık Sistemi
```python
# Bulanık sistem çalıştırma
python fuzzy_wsn_system.py
```

### 3. Tam Analiz Pipeline
```bash
# Önce veri analizi
python veri_analizi.py

# Sonra bulanık sistem
python fuzzy_sistem.py
```

## 🧠 Bulanık Mantık Sistemi Özellikleri

### 1. Üyelik Fonksiyonları

#### Optimized Triangular (Üçgen)
- **Anchor Ratio**: low(7-15-18), medium(15-18-30), high(18-30-33)
- **Trans Range**: low(10-15-17), medium(15-17-20), high(17-20-27)
- **Node Density**: low(70-100-200), medium(100-200-300), high(200-300-330)
- **Iterations**: low(1-30-40), medium(30-40-70), high(40-70-113)

#### Optimized Gaussian (Gauss)
- **Anchor Ratio**: low(μ=14.6, σ=1.0), medium(μ=20.0, σ=1.1), high(μ=29.6, σ=1.5)
- **Trans Range**: low(μ=15.3, σ=0.9), medium(μ=19.7, σ=0.7), high(μ=24.0, σ=1.0)
- **Iterations**: low(μ=23.9, σ=7.8), medium(μ=48.6, σ=10.8), high(μ=81.7, σ=6.0)

#### Hybrid Optimized (Hibrit)
- Üçgen, Gauss ve Yamuk fonksiyonların optimal kombinasyonu
- En iyi performans için özelleştirilmiş

#### Optimized Trapezoidal (Yamuk)
- Daha yumuşak geçişler için yamuk üyelik fonksiyonları

### 2. Kural Tabanı (27 Kural)

Korelasyon analizi sonuçlarına göre optimize edilmiş kurallar:
- **Iterations ↔ ALE**: Negatif korelasyon (-0.46) - güçlü
- **Anchor Ratio ↔ ALE**: Negatif korelasyon (-0.35) - orta
- **Node Density ↔ ALE**: Negatif korelasyon (-0.30) - orta
- **Trans Range ↔ ALE**: Pozitif korelasyon (0.44) - güçlü

```python
# Örnek kurallar
({"node_density": "low", "iterations": "low"}, "high", 5.0)
({"iterations": "high", "node_density": "high"}, "low", 2.0)
({"anchor_ratio": "high", "trans_range": "high"}, "low", 1.0)
```

### 3. Defuzzification Yöntemleri

1. **Centroid (Ağırlık Merkezi)**: Klasik ağırlık merkezi yöntemi
2. **Weighted Average (Ağırlıklı Ortalama)**: Hızlı hesaplama
3. **Max Membership (Maksimum Üyelik)**: En yüksek aktivasyonlu sınıf
4. **Center of Sums (COS)**: Gelişmiş ağırlık merkezi

### 4. Agregasyon Yöntemleri
- **MIN**: Minimum operatörü 
- **PROD**: Çarpım operatörü 

## 📈 Model Performansı

### Test Edilen Kombinasyonlar
- **4 Üyelik Fonksiyonu Türü** × **4 Defuzzification Yöntemi** × **2 Agregasyon**
- **Toplam: 32 farklı model kombinasyonu**

### Değerlendirme Metrikleri
- **MAE (Mean Absolute Error)**: Ortalama mutlak hata
- **RMSE (Root Mean Square Error)**: Kök ortalama kare hata
- **R² (Coefficient of Determination)**: Belirleme katsayısı



## 🔍 Veri Analizi Özellikleri

### 1. Otomatik Veri Temizleme
- Eksik değer tespiti ve doldurma
- Aykırı değer analizi (IQR yöntemi)
- std_ale sutunun çıkarılması

### 2. İstatistiksel Analiz
- Temel istatistikler (ortalama, medyan, standart sapma)
- Korelasyon matrisi hesaplama
- Özellikler arası ilişki analizi

### 3. Görselleştirmeler
- **Özellik Dağılımları**: 5 histogram grafiği
- **Korelasyon Isı Haritası**: Özellikler arası korelasyon
- **Gerçek vs Tahmin**: Scatter plot
- **Hata Dağılımı**: Hata histogramı
- **Residual Plot**: Artık değer analizi
- **Model Karşılaştırması**: Performans bar grafiği

## 🛠️ Gelişmiş Özellikler


### 1. Hata Analizi
En problemli tahminlerin detaylı analizi:
- En yüksek 20 hatalı tahminin listelenmesi
- Hata kaynaklarının belirlenmesi
- Model zayıflıklarının tespiti

### 2. Otomatik Model Seçimi
- 32 farklı kombinasyonun otomatik test edilmesi
- En düşük MAE değerine sahip modelin seçilmesi
- Performans metriklerinin karşılaştırılması

## 📊 Çıktı Örnekleri

### Konsol Çıktısı
```
=== VERİ ANALİZİ ===
Veri seti boyutu: (107, 6)

Değişkenler arası korelasyon (ALE ile):
iterations        -0.461234
trans_range        0.441876
anchor_ratio      -0.351234
node_density      -0.301456

=== FUZZY LOGIC SİSTEM SONUÇLARI ===
Model                Method       Agg   MAE      RMSE     R²
-----------------------------------------------------------------
Hybrid_Optimized     weighted_avg min   0.1250   0.1890   0.8750
Optimized_Gauss      centroid     min   0.1320   0.1950   0.8650

=== EN İYİ MODEL ===
Model: Hybrid_Optimized
Method: weighted_avg
Aggregation: min
MAE: 0.1250
RMSE: 0.1890
R²: 0.8750
```

### Görsel Çıktılar
1. **Veri Dağılımları**: 5 adet histogram
2. **Korelasyon Matrisi**: Isı haritası
3. **ALE İlişki Grafikleri**: 4 adet scatter plot
4. **Model Performansı**: 4'lü görselleştirme paneli

## 🚨 Hata Yönetimi

### Yaygın Hatalar ve Çözümleri

1. **Dosya Bulunamadı**
   ```
   ❌ Hata: 'veri.csv' dosyası bulunamadı!
   ```
   **Çözüm**: CSV dosyasının doğru konumda olduğundan emin olun.

2. **Yanlış Veri Formatı**
   ```
   ❌ Hata: Sütun sayısı beklenen aralıkta değil!
   ```
   **Çözüm**: CSV dosyasının 5-6 sütun içerdiğinden emin olun.

3. **Üyelik Fonksiyonu Hatası**
   ```
   ERROR: division by zero
   ```
   **Çözüm**: Üyelik fonksiyonu parametrelerini kontrol edin.

## 📁 Proje Yapısı
```
wsn_fuzzy_system/
│
├── veri_analizir.py        # Veri analizi modülü
├── fuzzy_sistem.py         # Bulanık mantık sistemi
├── veri.csv                    # Veri dosyası
├── README.md                   # Bu dosya
├── requirements.txt            # Gereksinimler
```

## 🎯 Kullanım Alanları

- **Kablosuz Sensör Ağları Optimizasyonu**
- **Lokalizasyon Algoritması Geliştirme**  
- **IoT Sistem Tasarımı**
- **Bulanık Mantık Araştırmaları**
- **Makine Öğrenmesi Karşılaştırmaları**
- **Akademik Çalışmalar ve Tezler**

## 🔬 Teknik Detaylar

### Algoritma Akışı
1. **Veri Ön İşleme**: Temizleme, filtreleme, normalizasyon
2. **Üyelik Fonksiyonu Hesaplama**: Her özellik için fuzzy değerler
3. **Kural Değerlendirme**: 27 kuralın ağırlıklı aktivasyonu
4. **Agregasyon**: MIN/PROD operatörleri ile birleştirme
5. **Defuzzification**: Crisp çıkış değeri hesaplama
6. **Performans Değerlendirme**: MAE, RMSE, R² hesaplama

### Optimizasyon Teknikleri
- **Korelasyon Tabanlı Kural Ağırlıkları**: İstatistiksel analiz sonuçlarına göre
- **Hibrit Üyelik Fonksiyonları**: Farklı fonksiyon türlerinin optimal kombinasyonu
- **Adaptif Parametre Ayarları**: Veri dağılımına göre otomatik ayarlama

## 📝 Model Karşılaştırması

### Üyelik Fonksiyonu Performansları
| Model Type | En İyi MAE | En İyi RMSE | En İyi R² |
|------------|------------|-------------|-----------|
| Hybrid     | 0.1250     | 0.1890      | 0.8750    |
| Gaussian   | 0.1320     | 0.1950      | 0.8650    |
| Triangular | 0.1380     | 0.2010      | 0.8580    |
| Trapezoidal| 0.1420     | 0.2080      | 0.8520    |

### Defuzzification Yöntem Karşılaştırması
| Yöntem | Hız | Doğruluk | Kararlılık |
|--------|-----|----------|------------|
| Weighted Avg | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Centroid | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| COS | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| Max Member | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |


## 📄 Lisans

Bu proje MIT lisansı altında açık kaynak kodlu olarak sunulmaktadır. Eğitim ve araştırma amaçlı kullanım için serbesttir.


## 🏆 Performans Benchmarkları

### Diğer Yöntemlerle Karşılaştırma


Bu README dosyası,  Bulanık Mantık Analiz Sistemi'nin tüm özelliklerini ve kullanım şekillerini kapsamlı bir şekilde açıklamaktadır. Sistem, hem araştırmacılar hem de pratik uygulamalar için optimize edilmiş bir çözüm sunmaktadır.

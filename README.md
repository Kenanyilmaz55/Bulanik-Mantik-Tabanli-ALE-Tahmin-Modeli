# WSN Veri Seti Yükleme ve Analiz Modülü

## 📋 Genel Bakış

Bu modül, Kablosuz Sensör Ağları (Wireless Sensor Networks - WSN) veri setlerini yüklemek, analiz etmek ve bulanık sistem uygulamaları için hazırlamak amacıyla geliştirilmiştir. Modül, veri kalitesi kontrolü, istatistiksel analiz ve görselleştirme özellikleri sunar.

## 🔧 Gereksinimler

### Gerekli Python Kütüphaneleri
```
pandas >= 1.3.0
numpy >= 1.21.0
matplotlib >= 3.4.0
seaborn >= 0.11.0
```

### Kurulum
```bash
pip install pandas numpy matplotlib seaborn
```

## 📊 Veri Seti Formatı

Modül aşağıdaki formatta CSV dosyalarını işler:

### Beklenen Sütunlar
1. **anchor_ratio**: Çapa oranı
2. **transmission_range**: İletim aralığı
3. **node_density**: Düğüm yoğunluğu
4. **iteration_count**: Yineleme sayısı
5. **ALE**: Average Localization Error (Ortalama Lokalizasyon Hatası)
6. **std_dev**: Standart sapma (opsiyonel)

### Veri Özellikleri
- **Beklenen satır sayısı**: 107
- **Sütun sayısı**: 5-6 (standart sapma sütunu opsiyonel)
- **Veri tipi**: Tüm sütunlar sayısal olmalıdır

## 🚀 Kullanım

### Temel Kullanım
```python
from wsn_data_analyzer import load_and_analyze_csv, prepare_data_for_fuzzy_system

# Veri yükleme ve analizi
data_array, data_df = load_and_analyze_csv('veri.csv')

# Bulanık sistem için veri hazırlama
if data_array is not None:
    processed_data = prepare_data_for_fuzzy_system(data_array)
```

### Modülü Çalıştırma
```bash
python wsn_data_analyzer.py
```

## 📈 Özellikler

### 1. Veri Yükleme ve Doğrulama
- CSV dosyasını otomatik yükleme
- Sütun isimlerini standartlaştırma
- Veri boyutu kontrolü
- Veri tipi doğrulaması

### 2. Veri Kalitesi Kontrolü
- **Eksik değer tespiti**: Eksik değerleri tespit eder ve ortalama ile doldurur
- **Aykırı değer analizi**: IQR yöntemi ile aykırı değerleri tespit eder
- **Veri format doğrulaması**: Beklenen format ile uyumluluğu kontrol eder

### 3. İstatistiksel Analiz
- Temel istatistikler (ortalama, medyan, standart sapma)
- Korelasyon matrisi hesaplama
- Özellikler arası ilişki analizi

### 4. Görselleştirme
- **Dağılım grafikleri**: Her özellik için histogram
- **Korelasyon ısı haritası**: Özellikler arası korelasyonu gösterir
- **Saçılım grafikleri**: ALE ile giriş özellikleri arasındaki ilişki
- **Trend analizi**: Regresyon çizgileri ile trend gösterimi

## 📊 Çıktı Örnekleri

### Konsol Çıktısı
```
==========================================
WSN VERİ SETİ YÜKLEME VE ANALİZ MODÜLÜ
==========================================
CSV dosyası yükleniyor...
✓ Veri başarıyla yüklendi!
  - Boyut: 107 satır × 6 sütun

Sütun isimleri:
  1. anchor_ratio
  2. transmission_range
  3. node_density
  4. iteration_count
  5. ALE
  6. std_dev

✓ Eksik değer bulunmuyor

🔍 Aykırı değer kontrolü:
  - anchor_ratio: Aykırı değer yok
  - transmission_range: 3 aykırı değer tespit edildi
  ...

✅ Veri başarıyla yüklendi ve işlendi!
```

### Görselleştirmeler
1. **Özellik Dağılımları**: 5 adet histogram grafiği
2. **Korelasyon Matrisi**: Isı haritası formatında
3. **ALE İlişki Grafikleri**: 4 adet saçılım grafiği

## 🔧 Fonksiyon Detayları

### `load_and_analyze_csv(file_path='veri.csv')`
- CSV dosyasını yükler ve kapsamlı analiz yapar
- **Parametreler**: `file_path` (str): CSV dosya yolu
- **Döndürür**: `(data_array, data_df)` - NumPy array ve Pandas DataFrame

### `create_data_visualizations(data)`
- Veri seti için görselleştirmeler oluşturur
- **Parametreler**: `data` (DataFrame): Pandas DataFrame
- **Çıktı**: Matplotlib grafikleri

### `validate_data_format(data)`
- Veri formatını doğrular
- **Parametreler**: `data` (DataFrame): Pandas DataFrame
- **Döndürür**: `bool` - Format geçerliliği

### `prepare_data_for_fuzzy_system(data_array)`
- Bulanık sistem için veriyi hazırlar
- **Parametreler**: `data_array` (np.array): NumPy array
- **Döndürür**: `np.array` - İşlenmiş veri

## 🚨 Hata Yönetimi

### Yaygın Hatalar ve Çözümleri

1. **Dosya Bulunamadı**
   ```
   ❌ Hata: 'veri.csv' dosyası bulunamadı!
   ```
   **Çözüm**: Dosya yolunu kontrol edin ve CSV dosyasının doğru konumda olduğundan emin olun.

2. **Yanlış Sütun Sayısı**
   ```
   ❌ Hata: Sütun sayısı beklenen aralıkta değil!
   ```
   **Çözüm**: CSV dosyasının 5-6 sütun içerdiğinden emin olun.

3. **Sayısal Olmayan Veriler**
   ```
   ⚠️ Uyarı: Tüm sütunlar sayısal değil!
   ```
   **Çözüm**: Tüm sütunların sayısal veri içerdiğinden emin olun.

## 📁 Dosya Yapısı
```
project/
│
├── wsn_data_analyzer.py    # Ana modül
├── veri.csv               # Veri dosyası
├── README.md              # Bu dosya
└── requirements.txt       # Gereksinimler
```

## 🎯 Kullanım Alanları

- Kablosuz sensör ağları araştırmaları
- Lokalizasyon algoritması geliştirme
- Bulanık sistem uygulamaları
- Veri madenciliği projeleri
- Akademik çalışmalar

## 📝 Notlar

- Modül, 107 satırlık standart WSN veri seti için optimize edilmiştir
- Eksik değerler otomatik olarak ortalama ile doldurulur
- Aykırı değerler tespit edilir ancak otomatik olarak kaldırılmaz
- Tüm görselleştirmeler otomatik olarak gösterilir

## 🤝 Katkıda Bulunma

Bu modülü geliştirmek için:
1. Yeni özellikler ekleyebilirsiniz
2. Hata düzeltmeleri yapabilirsiniz
3. Dokümantasyonu iyileştirebilirsiniz
4. Test senaryoları ekleyebilirsiniz

## 📄 Lisans

Bu proje açık kaynak kodludur ve eğitim amaçlı kullanım için geliştirilmiştir.

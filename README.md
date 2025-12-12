# 📊 EVDS Verilerine Python ile Erişim Kılavuzu

Bu repo, **Türkiye Cumhuriyet Merkez Bankası (TCMB)** tarafından sunulan **Elektronik Veri Dağıtım Sistemi (EVDS)** API’sine Python üzerinden nasıl erişileceğini adım adım gösterir.  

---

## 🎯 Amaç

Bu proje ile EVDS API üzerinden ekonomik verileri (örneğin döviz kurları, faiz oranları, endeksler vb.) Python kullanarak çekmeyi, düzenlemeyi ve analiz etmeyi öğreneceksin.

---

## 🧩 1️⃣ API Anahtarı Alma

1. [EVDS Resmî Sitesine Git →](https://evds2.tcmb.gov.tr/)
2. Giriş yap veya yeni bir hesap oluştur.
3. Profil sayfandan bir **API Key (anahtar)** oluştur.
4. Bu anahtarı kodda `YOUR_API_KEY` olarak kullanacaksın.

> 🔐 **Not:** API anahtarını paylaşma. Kodunu GitHub’a yüklerken gizli tut.

---

## ⚙️ 2️⃣ Gerekli Kütüphanelerin Kurulumu

Terminal veya Colab hücresine şu komutu yaz:

```bash
pip install evds pandas matplotlib
```

---


## 💻 4️⃣ Python Kod Örneği

Aşağıdaki kod, **2013’ten bugüne kadar EUR/TL kurunu** EVDS API üzerinden çekip CSV’ye kaydeder:

```python
from evds import evdsAPI
import pandas as pd
from datetime import date

# 1️⃣ EVDS API bağlantısı
api = evdsAPI("YOUR_API_KEY")

# 2️⃣ Veri çekme (EUR / TL kuru)
df = api.get_data(
    series=["TP.DK.EUR.A.YTL"],
    startdate="01-01-2013",
    enddate=date.today().strftime("%d-%m-%Y"),
)

# 3️⃣ Tarih formatlama
df["Tarih"] = pd.to_datetime(df["Tarih"], dayfirst=True, errors="coerce")
df = df.sort_values("Tarih")

# 4️⃣ CSV çıktısı oluşturma
df.to_csv("TP_DK_EUR_A_YTL_2013_today.csv", index=False, encoding="utf-8-sig")

# 5️⃣ İlk 5 satırı görüntüle
print(df.head())
```

---

## 📈 5️⃣ Grafikle Görselleştirme

Veriyi çizdirmek için `matplotlib` kütüphanesini kullanabilirsin:

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10,5))
plt.plot(df["Tarih"], df["TP.DK.EUR.A.YTL"], label="EUR/TL")
plt.title("EUR / TL Döviz Kuru (2013–Günümüz)")
plt.xlabel("Tarih")
plt.ylabel("Kur")
plt.grid(True)
plt.legend()
plt.show()
```

---

## 📊 6️⃣ Örnek Çıktı

| Tarih | TP.DK.EUR.A.YTL |
|-------|------------------|
| 2013-01-02 | 2.33 |
| 2013-01-03 | 2.34 |
| 2013-01-04 | 2.34 |
| ... | ... |

---

## 💡 7️⃣ Farklı Serilerle Denemeler

| Veri Türü | EVDS Kodu |
|------------|------------|
| USD / TL Döviz Kuru | `TP.DK.USD.A.YTL` |
| TÜFE (Genel) | `TP.FG.J0` |
| Faiz Oranları | `TP.KK.MB.AO` |
| Sanayi Üretim Endeksi | `TP.SUE.A` |

> 🔍 Farklı serileri öğrenmek için [EVDS Seri Pazarı](https://evds2.tcmb.gov.tr/index.php?/evds/serieMarket) sayfasına göz atabilirsin.

---

## 🧠 8️⃣ Sık Sorulan Sorular

**❓ API Key geçersiz diyor, ne yapmalıyım?**  
→ EVDS hesabından yeni bir anahtar oluştur. Kopyalarken boşluk kalmadığından emin ol.

**❓ Veri çekilmiyor, neden olabilir?**  
→ Seri kodunu doğru girdiğinden emin ol (büyük/küçük harf ve noktalama farkı önemlidir).

**❓ CSV dosyası Türkçe karakter hatası veriyor.**  
→ `encoding="utf-8-sig"` parametresi eklenmeli.

---

## 📚 Kaynaklar

- [📘 TCMB EVDS Resmî Sitesi](https://evds2.tcmb.gov.tr/)
- [📦 evds Python Paketi (PyPI)](https://pypi.org/project/evds/)





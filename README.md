# Optimizasyon Algoritmaları

Bu repo, dinamik programlama kullanılarak çözülen iki optimizasyon probleminin Python implementasyonlarını içermektedir:

1. **Müşteri Memnuniyeti Maksimizasyonu**: Genel memnuniyeti en üst düzeye çıkarmak için müşterilerin temsilcilere optimal atanması
2. **ROI Maksimizasyonu**: Bütçe kısıtı dahilinde yatırım getirisini en üst düzeye çıkarmak için kampanya seçimi

## Problem Açıklamaları

### Müşteri Memnuniyeti Maksimizasyonu

Bu algoritma, toplam memnuniyeti en üst düzeye çıkarmak için müşterilerin temsilcilere optimal atanmasını bulur. Her müşteri herhangi bir temsilciye atanabilir ve her temsilci birden fazla müşteriye hizmet verebilir. Her müşteri-temsilci ikilisi için memnuniyet değeri bir memnuniyet matrisinde sağlanır.

Algoritma, optimal çözümleri aşamalı olarak oluşturmak için dinamik programlama kullanır.

```python
def max_satisfaction(customers, representatives, satisfaction_matrix):
    n = len(customers)  # Müşteri sayısı
    m = len(representatives)  # Temsilci sayısı
    
    # DP tablosu
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    
    # Dinamik programlama tablosunu doldurma
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            dp[i][j] = max(dp[i-1][k] + satisfaction_matrix[i-1][j-1] for k in range(m))
    
    return max(dp[n])
```

### ROI Maksimizasyonu (Sırt Çantası Problemi)

Bu algoritma, bütçe kısıtı dahilinde yatırım getirisini maksimize etmek için pazarlama kampanyalarının bir alt kümesini seçer. Her kampanyanın bir maliyeti ve beklenen bir yatırım getirisi vardır. Algoritma, 0/1 sırt çantası problemi yaklaşımının bir varyantını kullanır.

```python
def max_roi(campaigns, budget):
    n = len(campaigns)
    dp = [[0] * (budget + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        cost, roi = campaigns[i-1]
        for b in range(budget + 1):
            if cost > b:
                dp[i][b] = dp[i-1][b]  # Kampanyayı ekleyemiyoruz
            else:
                dp[i][b] = max(dp[i-1][b], dp[i-1][b-cost] + roi)  # Seçmeli miyiz?
    
    return dp[n][budget]
```

## Kullanım Örneği

```python
# Örnek veri
customers = ["A", "B", "C"]
representatives = ["X", "Y"]
satisfaction_matrix = [
    [8, 6],  # A müşterisinin X ve Y temsilcileri için memnuniyet değeri
    [7, 9],  # B müşterisi
    [5, 8]   # C müşterisi
]

campaigns = [(3, 5), (2, 3), (5, 8), (4, 6)]  # Her kampanya için (maliyet, roi)
budget = 7

# Maksimum değerleri hesaplama
max_satisfaction_value = max_satisfaction(customers, representatives, satisfaction_matrix)
max_roi_value = max_roi(campaigns, budget)

print("Maksimum müşteri memnuniyeti:", max_satisfaction_value)
print("Maksimum yatırım getirisi (ROI):", max_roi_value)
```

## Zaman Karmaşıklığı

- **Müşteri Memnuniyeti**: O(n×m²) - n müşteri sayısı ve m temsilci sayısı
- **ROI Maksimizasyonu**: O(n×b) - n kampanya sayısı ve b bütçe

## Uygulamalar

Bu algoritmalar çeşitli iş senaryolarında uygulanabilir:

- Müşteri hizmetleri optimizasyonu
- Satış ekibi atamaları
- Kaynak tahsisi
- Pazarlama bütçesi optimizasyonu
- Yatırım portföyü yönetimi

## Gereksinimler

- Python 3.x

# TPMA-Stock-Analysis-CAPM-ARMA-GARCH

## 🚢 Analisis Risiko dan Peramalan Saham TPMA.JK

**Analisis risiko dan peramalan saham TPMA.JK (Trans Power Marine Tbk.)** menggunakan model **CAPM**, **ARMA**, dan **GARCH** dalam R. Perhitungan mencakup **beta**, *expected return*, *volatility forecasting*, dan **Value-at-Risk (VaR)** untuk keputusan investasi yang terinformasi.

Repository ini berisi analisis lengkap saham TPMA.JK menggunakan pendekatan kuantitatif. Tujuannya adalah menilai risiko sistematis, memodelkan *time series* return, dan meramalkan volatilitas.

---

## 📊 Sumber Data dan Spesifikasi

### Data Source: `tidyquant` Package (R)

Data historis harga saham diambil menggunakan *package* `tidyquant` dengan fungsi `tq_get()`.

* **Data Saham TPMA.JK:**
    ```R
    TPMA <- tq_get("TPMA.JK", get = "stock.prices", from = "2010-01-01", to = "2025-11-01")
    ```
* **Data IHSG (Market Benchmark ^JKSE):**
    ```R
    IHSG <- tq_get("^JKSE", get = "stock.prices", from = "2010-01-01", to = "2025-11-01")
    ```

### Variabel Utama yang Digunakan

* **Harga Penutupan (*Close*)** $\rightarrow$ Perhitungan *Return*
* ***Adjusted Close*** $\rightarrow$ Perhitungan *Return* yang sudah disesuaikan
* **Log Returns** $\rightarrow$ Dihitung menggunakan `periodReturn(type = "log")`

---

## 🛠️ Metodologi Analisis

| Tahapan Analisis | Model / Metode | Keterangan |
| :--- | :--- | :--- |
| **I. Analisis Risiko Sistematis** | **CAPM** | Menghitung **Beta** dan *Expected Return*. |
| | $\text{Expected Return} = R_f + \beta \times (R_m - R_f)$ | **Risk-Free Rate ($R_f$): 6%** (asumsi) |
| | **Beta** $(\beta)$ | $\text{Kovarians}(\text{Return Saham}, \text{Return Pasar}) / \text{Varians}(\text{Return Pasar})$ |
| **II. Pemodelan *Time Series*** | **ARMA(2,0,1)** | Memodelkan *Log Returns* untuk peramalan (*forecasting*). |
| | **Diagnostik** | *Stationarity Test* (ADF), ACF, PACF, *Ljung-Box test*. |
| **III. Pemodelan Volatilitas** | **GARCH** | Model **eGARCH(1,2)** dengan distribusi Student's t. |
| | **VaR Forecasting** | Estimasi kerugian maksimal (1% & 5% level signifikansi). |

---

## 📈 Hasil Utama dan Interpretasi

### 1. CAPM Results

| Metric | Value | Interpretasi |
| :--- | :--- | :--- |
| **Beta** ($\beta$) | **0.385** | **Rendah**. Saham **defensif**, kurang sensitif terhadap pergerakan pasar. |
| **Expected Return** | **5.46%** | *Return* yang disyaratkan oleh pasar berdasarkan risiko sistematis. |
| **Realized Return** | 4.33% | *Return* aktual historis. |
| **Alpha** ($\alpha$) | -0.0113 | **Negatif**. Kinerja aktual di bawah ekspektasi CAPM. |
| **Standard Deviasi** | 0.0398 | Volatilitas historis harian. |
| **Sharpe Ratio** | -0.42 | *Return* tidak mengkompensasi risiko dengan baik (jika menggunakan $R_f=6\%$). |

### 2. ARMA Model Performance

| Metric | Value |
| :--- | :--- |
| **MAE** | 0.0218 |
| **MSE** | 0.00157 |
| **AIC** | -11286.64 |

### 3. GARCH VaR Forecast (20 Hari ke Depan)

* **Volatilitas Teramalkan:** Meningkat dari **0.0417** $\rightarrow$ **0.0443** (Peningkatan volatilitas jangka pendek).
* **Analisis VaR:**
    * **VaR 1%** $\rightarrow$ Kerugian maksimal yang dapat terjadi dengan probabilitas **1%**
    * **VaR 5%** $\rightarrow$ Kerugian maksimal yang dapat terjadi dengan probabilitas **5%**
* **Tren:** Volatilitas meningkat bertahap, namun laju kenaikan cenderung menurun menjelang akhir periode. Risiko kerugian besar (VaR 1%) lebih ekstrem, sedangkan risiko kerugian moderat (VaR 5%) relatif stabil.

---

## 🎯 Implikasi Investasi

* **Karakteristik Saham TPMA:** **Beta rendah** (defensif), **Volatilitas rendah** (stabilitas harga tinggi).
* **Kesimpulan:** Cocok untuk **investor konservatif** yang memprioritaskan mitigasi risiko sistematis. *Return* yang dihasilkan relatif rendah, sejalan dengan risiko sistematisnya.
* **Rekomendasi:** **Diversifikasi** disarankan, berinvestasi pada saham *high-beta* atau aset lain dapat mengoptimalkan *risk-adjusted return* portofolio.

---

## ⚠️ Limitations & Disclaimer

* **Asumsi Model:** Analisis terikat pada asumsi model yang digunakan (e.g., *Market efficiency* dan *single period* dalam CAPM).
* **Data Historis:** Data historis **bukan jaminan** untuk hasil di masa depan (*futuro*).
* **Tujuan:** Repository ini murni untuk **edukasi** dan penelitian kuantitatif, **bukan rekomendasi investasi** finansial.

---

## 🔗 References

* Yahoo Finance (via `tidyquant` package)
* Tsay, R. S. (2010). *Analysis of Financial Time Series.*
* Engle, R. F. (1982). Autoregressive Conditional Heteroscedasticity (*ARCH*).
* Sharpe, W. F. (1964). Capital Asset Prices: A Theory of Market Equilibrium.

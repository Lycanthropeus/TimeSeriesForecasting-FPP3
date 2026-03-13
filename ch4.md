# Time series features

The `features` package can be used to calculate summary statistics from a time series.

<u>Example usage</u>:
To calculate the mean of trips across regions:
```
tourism |>
+  features(Trips, list(mean = mean)) |>
+  arrange(mean)
```

To calculate the quantiles for trip counts across regions:
```
tourism |> features(Trips, quantile)
```

## ACF features

The `feat_acf()` function computes the following six or seven features:

- the first autocorrelation coefficient from the original data;
- the sum of squares of the first ten autocorrelation coefficients from the original data;
- the first autocorrelation coefficient from the differenced data;
- the sum of squares of the first ten autocorrelation coefficients from the differenced data;
- the first autocorrelation coefficient from the twice differenced data;
- the sum of squares of the first ten autocorrelation coefficients from the twice differenced data;
- For seasonal data, the autocorrelation coefficient at the first seasonal lag is also returned.

## STL features

$$
F_T = \max\left(0, 1 - \frac{Var(R_t)}{Var(R_t + T_t)}\right)
$$
If the series is strongly trended, the denominator $y_t - S_t$ will have more variance than $R_t$ alone. So $F_T$ will be close to 1 for strongly trended series, and close to zero if not.

$$
F_S = \max\left(0, 1 - \frac{Var(R_t)}{Var(R_t + S_t)}\right)
$$

If the series is strongly seasonal, the denominator $y_t - T_t$ will have more variance than $R_t$ alone. So $F_S$ will be close to 1 for strongly seasonal series, and close to zero if not.


Some more useful features of `feat_stl`:
- `seasonal_peak_year` indicates the timing of the peaks — which month or quarter contains the largest seasonal component. 
- `seasonal_trough_year` indicates the timing of the troughs — which month or quarter contains the smallest seasonal component.
- `spikiness` measures the prevalence of spikes in $R_t$
- `linearity` measures the linearity of the trend component of the STL decomposition. It is based on the coefficient of a linear regression applied to the trend component.
- `curvature`
- `stl_e_acf1` is the first autocorrelation coefficient of the remainder series.
- `stl_e_acf10` is the sum of squares of the first ten autocorrelation coefficients of the remainder series.


Some useful features of `feasts`:

- `coef_hurst` will calculate the Hurst coefficient of a time series which is a measure of “long memory”. A series with long memory will have significant autocorrelations for many lags.
- `feat_spectral` will compute the (Shannon) spectral entropy of a time series, which is a measure of how easy the series is to forecast. A series which has strong trend and seasonality (and so is easy to forecast) will have entropy close to 0. A series that is very noisy (and so is difficult to forecast) will have entropy close to 1.
`box_pierce` gives the Box-Pierce statistic for testing if a time series is white noise, and the corresponding p-value. This test is discussed in Section 5.4.
- `ljung_box` gives the Ljung-Box statistic for testing if a time series is white noise, and the corresponding p-value. This test is discussed in Section 5.4.
- `unitroot_kpss` gives the Kwiatkowski-Phillips-Schmidt-Shin (KPSS) statistic for testing if a series is stationary, and the corresponding p-value. This test is discussed in Section 9.1.
- `guerrero` computes the optimal λ value for a Box-Cox transformation using the Guerrero method (discussed in Section 3.1).

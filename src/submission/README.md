## Submission History

<p align="center">
  <img src="./history.png" />
</p>

| 檔名                                                         | 方法                                                                                                           | 上傳時間            | 評估結果  | 排名  |
| :----------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------- | :------------------ | :-------- | :---- |
| rule-based-lat-lon-module-capacity                           | 將歷史資料基於經緯度、模組及裝置容量分組，計算歷史 AR[^1] 用於推算未來發電量                                   | 2022-06-12 00:03:24 | 278.91238 |       |
| rule-based-lat-lon                                           | 將歷史資料基於經緯度分組，計算歷史 AR 用於推算未來發電量                                                       | 2022-06-13 23:37:04 | 277.33373 |       |
| rule-based-lat-lon(remove-outlier)                           | 基於經緯度分組，剔除輻射及發電量的離群值後再計算歷史 AR 推算發電量                                             | 2022-06-15 21:24:07 | 279.49857 |       |
| rule-based-lat-lon-module-capacity(remove-outlier)           | 基於經緯度、模組及裝置容量分組，剔除輻射及發電量的離群值後再計算歷史 AR 推算發電量                             | 2022-06-15 21:31:39 | 283.51476 |       |
| rule-based-lat-lon(remove-outlier-irradiance)                | 基於經緯度分組，剔除輻射量的離群值後再計算歷史 AR 推算發電量                                                   | 2022-06-16 22:02:26 | 278.37551 |       |
| xgb-lat-lon-module-capacity-1d                               | 基於經緯度、模組及裝置容量分組，再以預測日的觀測輻射[^2] 及晴空輻射[^3] 為輸入變數[^4] 訓練 XGboost 預估發電量 | 2022-06-19 21:38:59 | 271.40773 |       |
| xgb-lat-lon-module-capacity-prev3d                           | 基於經緯度、模組及裝置容量分組，再以預測日及過去 3 日的輻射[^5] 為輸入變數訓練 XGboost 預估發電量              | 2022-06-19 23:22:40 | 302.62303 |       |
| xgb-lat-lon-module-capacity(remove-outlier)                  | 基於經緯度、模組及裝置容量分組，剔除輻射及發電量的離群值後，再以前述資料[^4] 為輸入變數訓練 XGboost 預估發電量 | 2022-06-19 23:33:54 | 269.38945 |       |
| xgb-lat-lon-module-capacity(remove-outlier)(fix)             | 承接前個實驗，僅以 rule-based 方法替換 AUO PM060MW3 325W[^6] 的估計值                                          | 2022-06-19 23:56:02 | 258.79723 |       |
| xgb-lat-lon-module-capacity(remove-outlier)(normalized)(fix) | 承接前個實驗，僅在訓練模型的時候先將發電量除以裝置容量正規化，產出估計值以後再乘以裝置容量                     | 2022-06-20 23:03:25 | 258.46331 |       |
| xgb-lat-1d(remove-outlier)(normalized)                       | 基於緯度分組，再以前述資料[^4] 為輸入變數訓練 XGboost 預估發電量                                               | 2022-06-20 23:19:42 | 277.34923 |       |
| xgb-lat-1d(remove-outlier)(normalized)(fix)                  | 承接前個實驗，僅以 rule-based 方法替換 AUO PM060MW3 325W 的估計值                                              | 2022-06-20 23:20:33 | 276.31831 |       |
| xgb-lon-1d(remove-outlier)(normalized)(fix)                  | 承接前個實驗，僅改為基於經度分組                                                                               | 2022-06-20 23:27:59 | 269.63295 |       |
| xgb-lon-1d-prev1d-capacity(remove-outlier)(normalized)       | 基於經度分組訓練模型，並在輸入變數加入裝置容量[^7] 及過去 1 日的輻射[^8]                                       | 2022-06-21 22:53:40 | 238.85634 |       |
| xgb-lon-1d-prev1d-capacity(hpo1)                             | 承接前個實驗，進行大量模型超參數調整                                                                           | 2022-06-21 23:40:28 | 241.13532 |       |
| xgb-lon-1d-prev1d-capacity(hpo2)                             | 承接前個實驗，進行小量模型超參數調整                                                                           | 2022-06-21 23:49:47 | 242.54411 |       |
| xgb-1d-prev1d-ensemble-lat-lon                               | 集合基於經度與緯度分組訓練的預測結果                                                                           | 2022-06-21 23:54:09 | 240.98290 |       |
| xgb-lon-1d-prev1d-capacity(remove-outlier)(normalized)       | 使用歷史最佳答案作為最終答案                                                                                   | 2022-06-21 23:55:00 | 238.85634 | 9/179 |

[^1]: Array to Inverter Ratio
[^2]: 外部資料來源：中央氣象局觀測資料查詢系統
[^3]: 外部資料來源：使用套件 [pvlib-python](https://github.com/pvlib/pvlib-python) 計算晴空輻射
[^4]: 輸入變數：逐小時 24 筆觀測輻射、逐小時 24 筆晴空輻射及日期參數 DayOfYearTransformed
[^5]: 輸入變數：逐小時 96 筆觀測輻射、逐小時 96 筆晴空輻射及日期參數 DayOfYearTransformed
[^6]: 該模組的訓練資料僅有 20 筆，訓練出來的模型泛化能力不足
[^7]: 基於經度或緯度分組雖然可以大幅增加每個群組的資料量，但由於群組裡面發電廠的位置都很接近，作為輸入變數的觀測輻射取自相同的氣象局觀測站，使得模型無法從輸入變數辨別不同的發電廠，加入裝置容量使模型能夠辨別它們
[^8]: 輸入變數：逐小時 48 筆觀測輻射、逐小時 48 筆晴空輻射、裝置容量 Capacity 及日期參數 DayOfYearTransformed

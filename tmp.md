在 dB（分贝）尺度中，功率比用下面的公式表示：
$
\text{dB}=10\log_{10}\!\left(\frac{P_{\text{out}}}{P_{\text{ref}}}\right)
$
当参考功率为 1 mW 时，单位叫 dBm，即  
$
\text{dBm}=10\log_{10}\!\left(\frac{P}{1\ \text{mW}}\right)
$
要求出对应 +3 dBm 的功率：
$
+3\ \text{dBm}=10\log_{10}\!\left(\frac{P}{1\ \text{mW}}\right)=3
$
解得

$
\frac{P}{1\ \text{mW}} = 10^{3/10}=10^{0.3}\approx 2.00
$

于是  
$
P \approx 2 \times 1\ \text{mW}=2\ \text{mW}
$

为什么说 +3 dBm 对应功率翻倍

* 3 dB 正好是使功率比为 2 的值，因为  
  $
  10\log_{10}2 = 3.0103\ \text{dB}
  $
* 在实际使用中，人们常把 +3 dB ≈ 功率翻倍 作为近似记忆法，忽略了 0.01 dB 的细微差别。
因此 +3 dBm 表示功率大约是参考功率的两倍（约 2 mW），这就是为什么 +3 dB（或 +3 dBm）对应功率乘以两倍的原因。

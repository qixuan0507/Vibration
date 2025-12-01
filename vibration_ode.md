# 震動與加速規：二階彈簧阻尼系統推導

## 系統模型與ODE建立
令質量 $m$、阻尼 $c$、剛度 $k$，位移為 $x(t)$，外力為 $f(t)$。由牛頓第二定律得
$$m\ddot{x}(t)+c\dot{x}(t)+kx(t)=f(t).$$
定義自然頻率 $\omega_n=\sqrt{k/m}$、阻尼比 $\zeta=c/(2\sqrt{mk})$，可化為
$$\ddot{x}+2\zeta\omega_n\dot{x}+\omega_n^2 x=\frac{1}{m}f(t).$$

## 拉普拉斯域表示與轉移函數
令 $\mathcal{L}\{x(t)\}=X(s)$、$\mathcal{L}\{f(t)\}=F(s)$，得
$$X(s)=H(s)F(s),\quad H(s)=\frac{1}{ms^2+cs+k}=\frac{1/k}{\left(\tfrac{s^2}{\omega_n^2}+\tfrac{2\zeta s}{\omega_n}+1\right)}.$$

## 自由振動解析解步驟（$f(t)=0$）
1. 解特徵方程 $r^2+2\zeta\omega_n r+\omega_n^2=0$，其根為
$$r_{1,2}=-\zeta\omega_n\pm\omega_n\sqrt{\zeta^2-1}.$$
2. 依阻尼比分類求通解：
   - **$0<\zeta<1$（欠阻尼）**：令 $\omega_d=\omega_n\sqrt{1-\zeta^2}$，
     $$x_h(t)=e^{-\zeta\omega_n t}\big(C_1\cos \omega_d t+C_2\sin \omega_d t\big).$$
     初始條件 $x(0)=x_0$、$\dot{x}(0)=v_0$，得到
     $$C_1=x_0,\qquad C_2=\frac{v_0+\zeta\omega_n x_0}{\omega_d}.$$
     因此
     $$x_h(t)=e^{-\zeta\omega_n t}\left[x_0\cos \omega_d t+\frac{v_0+\zeta\omega_n x_0}{\omega_d}\sin \omega_d t\right].$$
   - **$\zeta=1$（臨界阻尼）**：
     $$x_h(t)=\big(x_0+(v_0+\omega_n x_0)t\big)e^{-\omega_n t}.$$
   - **$\zeta>1$（過阻尼）**：令 $r_{1,2}$ 為上式兩根（皆為負實數），
     $$x_h(t)=C_1 e^{r_1 t}+C_2 e^{r_2 t},\quad
       \begin{cases}
       C_1=\dfrac{v_0-r_2 x_0}{r_1-r_2},\\[6pt]
       C_2=\dfrac{-v_0+r_1 x_0}{r_1-r_2}.
       \end{cases}$$
   - **$\zeta=0$（無阻尼）**：
     $$x_h(t)=x_0\cos \omega_n t+\frac{v_0}{\omega_n}\sin \omega_n t.$$

## 正弦強迫振動與穩態解
令 $f(t)=F_0\cos(\omega t)$，假設特解 $x_p(t)=X\cos(\omega t-\phi)$，代回可得振幅與相位
$$X(\omega)=\frac{F_0/k}{\sqrt{\big(1-(\tfrac{\omega}{\omega_n})^2\big)^2+\big(2\zeta\tfrac{\omega}{\omega_n}\big)^2}},\quad
\phi=\tan^{-1}\frac{2\zeta\tfrac{\omega}{\omega_n}}{1-(\tfrac{\omega}{\omega_n})^2}.$$ 
完整解為 $x(t)=x_h(t)+x_p(t)$，其中 $x_h$ 為瞬態項，會以 $e^{-\zeta\omega_n t}$ 衰減。

## 加速度與加速規量測
加速度為
$$\ddot{x}(t)=-\omega^2 X\cos(\omega t-\phi)-2\zeta\omega_n \omega X\sin(\omega t-\phi)$$
（第一項為穩態主導，第二項在欠阻尼時可忽略）。穩態正弦時，振幅 $A_{\text{acc}}=\omega^2 X$，加速規直接量得此量。若已知量測加速度，可反推位移振幅 $X=A_{\text{acc}}/\omega^2$。

## 轉換為一階狀態方程便於數值解
令 $x_1=x,\;x_2=\dot{x}$，則
\[\dot{\mathbf{x}}=\begin{bmatrix}0&1\\-\omega_n^2&-2\zeta\omega_n\end{bmatrix}\mathbf{x}+\begin{bmatrix}0\\\tfrac{1}{m}\end{bmatrix}f(t),\qquad
\mathbf{x}(0)=\begin{bmatrix}x_0\\v_0\end{bmatrix}.\]
以 Runge–Kutta 等 ODE 求解器步進即可獲得位移、速度與加速度（$\ddot{x}$）。

## 實務小結
- 模型核心為二階常係數線性 ODE；阻尼比決定瞬態形態。
- 自由振動解由特徵方程取得；強迫振動穩態解由頻率響應可直接寫出。
- 加速規量得的加速度振幅等於 $\omega^2$ 倍的位移振幅，可用於反推結構響應。

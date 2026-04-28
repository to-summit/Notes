## From dataset itself

1. Time series dataset: at a specified time point, the data itself is meaningless, whose meaning come from the relationship with adjacent time steps.
2. Locality: the recent data usually display more importance for predicted time step.
## Transformer's characteristic

Transformer's self-attention can grasp relationship in two point even their position is distant, which means it can model better in nonlinear and long time dependency which also imply they will easily fit the noisy in time series data.

## DLinear's characteristic 
1. Simple $\rightarrow$ they are not easy to overfit
2. Less (parameters) $\rightarrow$ means the most train will focus on fitting **true** pattern.
3. Explicitly model $trend + residual$, more efficient than implicitly learn by model itself.

### Complement
1. multi-step forecast: PatchTST(improved transformer) perform better.
2. Long Long series, dlinear don't have advantage.
3. Complex nonlinear relationship.

## Problems

1. How to proof the locality expressed in time series data ? 
2. Why we always use MA to extract the trend information ? 
3. $series = trend + cycle$ How to proof ? 
4. Indeed many improved transformer model also explicitly decompose but why they can't do better ? 
5. Since $series = trend + cycle$ is right, why the dlinear's advantage can't retain when forecast multi-step ? And now that this formula is the **true** internal pattern, why there in super-long series also discourage ?
## Statistical model

1. MA model expresses the current value of a **time series** as a linear function of current and past random shocks(error terms) with finite lag length
2. AR model specifies output variables that are dependent linearly on their own previous values on a stochastic basis.

## Statistical explanation to common models

- 1D CNNs is FIR(Finite Impulse Response)
> FIR only depends on recent **inputs**, no feedback from past **outputs**, end after **finite** steps
- RNN, LSTM are IIR(Infinite)

## Mold's decomposition

1. Stationary(weak): mean is constant and  covariance only related to $\Delta t$ so that **lag operator** on Hilbert space is isometric.
2. Zero mean: $E[X_t] = 0$.
3. Discrete time, classical for discrete TSF.
4. Forecaster is linear function of past value.

## Markov process

1. Defined on probably space
2. Mesurable
3. $P(X_{t+1}​=x_t+1​∣X_t​=x_t​,X_{t−1}​=x_{t−1}​,…,X_0​=x_0​)=P(X_{t+1}​=x_{t+1}​∣X_t​=x_t​)$

## Hilbert Space
1. Time series is a **string** of vector in Hilbert space $L^2(\Omega,\mathcal{F},\mathbb{P})$
- Inner product: $\langle X, Y \rangle = \mathbb{E}[X Y]$
- Metric: $\left|\left|X\right|\right| = \sqrt{E[X^2]}$ 
- Second moment is finite: $\mathbb{E}[X^2] \lt \infty$   

## In a perspective of **Hilbert space**, we can redefine the **TSF**:  
1. Linear regression $\rightarrow$ linear composition
2. $\min\left(\mathbb{E}\left[ \left( X_t - \hat{X}_t \right)^2 \right]\right)$ $\mapsto$ $\min\left(\left\| X_t - \hat{X}_t \right\|^2\right)$, which means our goal is find a vector in the $H_{t-1}$, which is formed by all the history $X_i (i \in {0, \cdots, t-1})$, can make the **error vector** satisfies $\langle X_i - \hat{X_t}, Y\rangle = 0$, which means $\mathbb{E}\left[(X_t- \hat{X_t})Y\right] = 0$ that is a system of equation the so-called **Yule-Walker equation**
$$\hat{X}_t = \phi_1 X_{t-1} + \cdots + \phi_p X_{t-p}$$
$$\mathbb{E}\left[(X_t - \hat{X}_t) X_{t-k}\right] = 0, \quad k=1,2,\dots,p$$
$$\mathbb{E}\left[\left(X_t - \sum_{i=1}^p \phi_i X_{t-i}\right) X_{t-k}\right] = 0$$
$$\mathbb{E}\left[X_t X_{t-k}\right] - \sum_{i=1}^p \phi_i \mathbb{E}\left[X_{t-i} X_{t-k}\right] = 0$$
$$\gamma(k) = \sum_{i=1}^p \phi_i \gamma(k-i), \quad k=1,2,\dots,
$\begin{bmatrix} \gamma(0) & \gamma(1) & \cdots & \gamma(p-1) \\ \gamma(1) & \gamma(0) & \cdots & \gamma(p-2) \\ \vdots & \vdots & \ddots & \vdots \\ \gamma(p-1) & \gamma(p-2) & \cdots & \gamma(0) \end{bmatrix} \begin{bmatrix} \phi_1 \\ \phi_2 \\ \vdots \\ \phi_p \end{bmatrix} = \begin{bmatrix} \gamma(1) \\ \gamma(2) \\ \vdots \\ \gamma(p) \end{bmatrix}$$

workflow: linear forecast -> orthogonal projection -> Yule-Walker equation -> coefficient only relative to ACF
3. lag operator $B$ is **isometric linear operator**, isometric proofed by $\left|\left|BX\right|\right| = \left|\left|X\right|\right|$


## Fourier Transformer 
1. Definition： $$\hat{x}(\omega) = \int_{-\infty}^{\infty}x(t)e^{-j\omega t}dt$$
2. $$|\mathcal{F}\left[\dot{x}\right]| \propto \omega \cdot |\hat{x}(\omega)|$$

	**Proof**: 
		$$\mathcal{F}\left[\dot{x}(t)\right] = \int_{-\infty}^{\infty}\frac{dx}{dt}\left(t\right)e^{-j\omega t}dt$$
		Define  
				$$u = e^{-j\omega t} \Rightarrow du = -j\omega e^{-j\omega t}dt$$
		and   
				$$dv = \dot{x}(t)dt \Rightarrow v =x(t)$$
		substitute:
				 $$\int udv = \left.uv \right|_{-\infty}^{\infty} - \int vdu$$
				 $$\int_{-\infty}^{\infty}\dot{x}e^{-j\omega t}dt = \left. x(t) e^{-j\omega t}\right|_{-\infty}^{\infty} + j\omega\int_{-\infty}^{\infty}x(t)e^{-j\omega t}dt$$
		when 
				 $$t \rightarrow \pm\infty, x(t) \rightarrow 0$$
		so 
				 $$\int\dot{x}e^{-j\omega t}dt = j\omega\int x(t)e^{-j\omega t}dt$$
		 $j$  is phrase, no effect on ampltitude.
3. noisy implies high fluctuations and fast vary so we need to use more high $\omega$ components to fit the raw curve so that in transformered curve $\hat{x}(\omega) \ne 0\text{ in high } \omega$
4.  If we do the FT on TS data, we can see frequency concentrate on low rather high, which implies the true data can be modeled by linear model.(Why low frequency can be modeled by linear)
## Relation
1. **Lag operator B can be seen as phase moving in frequency domain**

   $$
   x_t = \cos(\omega t) = \Re(e^{i\omega t})
   $$

   $$
   Bx_t = \cos(\omega(t-1)) = \Re(e^{i\omega(t-1)}) = \Re(e^{-i\omega}e^{i\omega t})
   $$

2. **ARMA can be viewed as a Filter in spectrum in the perspective of spectral analysis:**

  ARMA:
   $$
   (1 - \phi_1 B - \cdots - \phi_p B^p)X_t = (1 + \theta_1 B + \cdots + \theta_q B^q)\varepsilon_t
   $$

  Simplified as:
   $$
   \Phi(B)X_t = \Theta(B)\varepsilon_t
   $$

   In frequency domain:
   $$
   \Phi(e^{-i\omega})X(\omega) = \Theta(e^{-i\omega})\varepsilon(\omega)
   $$

   So:
   $$
   X(\omega) = \frac{\Theta(e^{-i\omega})}{\Phi(e^{-i\omega})}\varepsilon(\omega)
   $$

   Spectrum:
   $$
   S_X(\omega) = \left|\frac{\Theta(e^{-i\omega})}{\Phi(e^{-i\omega})}\right|^2 S_\varepsilon(\omega)
   $$

   > So, what the ARMA do is multiply a complex weight on each frequency ω so that some frequency more stronger while other weak, which is the so-called filter in spectral analysis.
	
3. Linear model, which in the perspective of spectral analysis is a low-pass filter, only have little parameters so can't fit the high frequency but low energy noisy. But Transformer/LSTM/... these models is non-linear and have more capacity with more freedom to fit anything including noisy. For low the **loss function** they inevitably take the noisy into model.

4. Here, we can do a small **conclusion**. There are two perspectives to view the TSF. First, in spectral analysis, the linear model is a **low-pass filter** which can't fit high-frequency but low-energy noisy. You can demonstrate it is a complex weight, which is related to **lag operator** of frequencies. While, transformer and other complex model will have enough competence to fit the dramatically varied noisy for low the **loss function**. Second, we can view the time series data as a vector in Hilbert space, then the **lag operator** is a linear isometric operator, it represents a linear transform on the vector. The magic is we can understand the TSF as find a forecasted vector in the past whose error vector is **orthogonal** with itself. Then comes **Wold's decomposition** which give a theorical limit of linear forecasting in **time domain**. But **Kolmogorov equation** give a theorical limit in the **frequency domain**. The difference is Wold just give the decision there is a theorical limit, but Kolmogorov give the outcome: $$\sigma_\epsilon^2 = 2\pi exp\left(\frac{1}{2\pi}\int_{-\pi}^{\pi}log f(\omega)d\omega\right)$$ where the $f(\omega)$ is spectrum density function can be given by: $$ f(\omega) = \frac{1}{2\pi}|H(\omega)|^2\sigma_\epsilon^2,   H(\omega) = \Phi(B)$$
	
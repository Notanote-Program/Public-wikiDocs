<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8"/>
  <title>椭圆切割策略的完整数学阐述</title>

  <!-- KaTeX -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.css"/>
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.js"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/contrib/auto-render.min.js"
          onload="renderMathInElement(document.body, {delimiters: [
            {left: '$$', right: '$$', display: true},
            {left: '$',  right: '$',  display: false}
          ]});"></script>

  <!-- 简单样式，让阅读更舒适 -->
  <style>
    body {
      font-family: "Times New Roman", serif;
      line-height: 1.6;
      max-width: 800px;
      margin: 40px auto;
      padding: 0 20px;
      color: #222;
      background: #fafafa;
    }
    h1, h2, h3 { color: #005a9c; }
    hr { border: none; border-top: 1px solid #ccc; margin: 2em 0; }
  </style>
</head>

<body>
  <h1>椭圆切割策略的完整数学阐述</h1>
  <p>（面向工业加工机的“斜率变化率”加密切片法）</p>

  <hr>

  <h2>1. 椭圆参数化</h2>
  <p>取标准椭圆</p>
  $$ \mathcal E:\quad \frac{x^{2}}{a^{2}}+\frac{y^{2}}{b^{2}}=1 $$
  <p>用离心角 $\theta \in [0,2\pi)$ 参数化</p>
  $$ x(\theta)=a\cos\theta,\qquad y(\theta)=b\sin\theta $$
  <p>该参数化的优势是：</p>
  <ul>
    <li>每改变单位角度 $\Delta\theta$，在弧长意义上并非等距，但能直接给出解析切线；</li>
    <li>$\theta$ 与“工程坐标” $(x,y)$ 的映射一一对应，方便后续离散采样。</li>
  </ul>

  <hr>

  <h2>2. 切线及其斜率</h2>
  <p>在任意 $\theta$ 处的切线方程为</p>
  $$ \frac{x\cos\theta}{a}+\frac{y\sin\theta}{b}=1 $$
  <p>其斜率</p>
  $$ k(\theta)=\frac{\mathrm dy}{\mathrm dx}\Big|_{\theta}=-\frac{b}{a}\cot\theta $$
  <p>这是一个单变量函数，完全由 $\theta$ 决定。<br>
     <strong>几何意义</strong>：$\cot\theta$ 在 $\theta\to 0^{+}$ 时趋于 $+\infty$，对应椭圆最右端切线几乎垂直；$\theta\to\frac{\pi}{2}^{-}$ 时 $\cot\theta\to 0$，切线趋近水平。</p>

  <hr>

  <h2>3. 斜率变化率（曲率映射）</h2>
  <p>为了度量“方向变化剧烈程度”，对 $k(\theta)$ 再求导：</p>
  $$ \frac{\mathrm dk}{\mathrm d\theta}=\frac{b}{a}\csc^{2}\theta $$
  <ul>
    <li>$\csc^{2}\theta\ge 1$，且在 $\theta=0,\pi$ 处趋于 $+\infty$；</li>
    <li>该导数越大，意味着极小的角度扰动将导致切线方向大幅旋转——工业机在此区域加工时易出现“抖动”或“过切”，因此需要更密集的切割。</li>
  </ul>

  <hr>

  <h2>4. 密度函数与切割间距</h2>
  <p>定义密度函数</p>
  $$ \rho(\theta)=\left|\frac{\mathrm dk}{\mathrm d\theta}\right|=\frac{b}{a}\csc^{2}\theta $$
  <p>归一化后</p>
  $$ f(\theta)=\frac{\rho(\theta)}{\displaystyle\int_{0}^{2\pi}\rho(\phi)\,\mathrm d\phi}= \frac{\csc^{2}\theta}{2\cot\theta\big|_{0}^{\pi}}=\frac{1}{2}\csc^{2}\theta $$
  <p>于是，对任意弧段 $[\theta_{1},\theta_{2}]$ 内所需切割次数正比于</p>
  $$ \int_{\theta_{1}}^{\theta_{2}}f(\theta)\,\mathrm d\theta $$
  <p><strong>结论</strong>：</p>
  <ul>
    <li>左右端（$\theta\approx 0,\pi$）密度最大，切割最密；</li>
    <li>上下端（$\theta\approx \pi/2,3\pi/2$）密度最小，切割最稀。</li>
  </ul>

  <hr>

  <h2>5. 离散采样算法（映射到水平线切割）</h2>
  <p>在代码实现中，切割工具仅支持“水平线” $y=\mathrm{const}$。<br>
     由 $y=b\sin\theta$ 反解</p>
  $$ \theta=\arcsin\frac{y}{b} $$
  <p>于是斜率变化率对 $y$ 的导数</p>
  $$ \frac{\mathrm dk}{\mathrm dy}=\frac{\mathrm dk}{\mathrm d\theta}\frac{\mathrm d\theta}{\mathrm dy}
     =\frac{b}{a}\frac{\csc^{2}\theta}{b\cos\theta}
     =\frac{1}{a\cos\theta\sin^{2}\theta} $$
  <ul>
    <li>当 $|\cos\theta|\to 0$（即 $y\to\pm b$），$\frac{\mathrm dk}{\mathrm dy}\to\infty$，需在极值附近加密水平线；</li>
    <li>当 $|\cos\theta|\approx 1$（即 $y\approx 0$），$\frac{\mathrm dk}{\mathrm dy}$ 最小，水平线可稀疏。</li>
  </ul>
</body>
</html>

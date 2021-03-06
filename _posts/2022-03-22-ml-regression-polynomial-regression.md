---
keywords: fastai
description: 2022.03.20 
title: "[MachineLearning] Regression Analysis - Polynomial Regression"
toc: false
badges: false
comments: false
categories: [regression analysis]
hide_{github,colab,binder,deepnote}_badge: true
nb_path: _notebooks/ml-regression-polynomial-regression.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/ml-regression-polynomial-regression.ipynb
-->

<div class="container" id="notebook-container">
        
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p><strong>Content creators:</strong> 이중원</p>
<p><strong>Content reviewers:</strong></p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="1.-Overview">1. Overview<a class="anchor-link" href="#1.-Overview"> </a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Polynomial-Regression&#46976;?">Polynomial Regression&#46976;?<a class="anchor-link" href="#Polynomial-Regression&#46976;?"> </a></h3><ul>
<li><p>데이터의 분포가 선형이 아닌 곡선으로 나타나는 경우에 사용하는 회귀</p>
</li>
<li><p>1차항이 아닌 2차, 3차항 등으로 확장하여 구성된 다항 회귀식</p>
</li>
<li><p>$\bar y = b_0 + {b_1}{x_i} + {b_2}{x_i^2} \cdot\cdot\cdot b_p x_i^p$</p>
</li>
</ul>
<h2 id="&#50508;&#44256;&#47532;&#51608;">&#50508;&#44256;&#47532;&#51608;<a class="anchor-link" href="#&#50508;&#44256;&#47532;&#51608;"> </a></h2><ul>
<li><p>입력 데이터셋을 X라 할 때,</p>
</li>
<li><p>입력 데이터셋에 새로운 변수로 추가하고, 이 확장된 변수를 포함한 데이터셋을 선형모델로 훈련시키는 방법</p>
</li>
<li><p>즉, X 를 통해 X^2, X^3 을 생성한 뒤, y = 1 + aX + bX^2 + cX^3 형식을 구성하고</p>
</li>
<li><p>a, b, c 를 선형 회귀 모델을 통해 학습하는 방법입니다.</p>
</li>
</ul>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="2.-Example">2. Example<a class="anchor-link" href="#2.-Example"> </a></h1>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#45936;&#51060;&#53552;&#49483;-&#51456;&#48708;">&#45936;&#51060;&#53552;&#49483; &#51456;&#48708;<a class="anchor-link" href="#&#45936;&#51060;&#53552;&#49483;-&#51456;&#48708;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.preprocessing</span> <span class="kn">import</span> <span class="n">PolynomialFeatures</span>
<span class="kn">from</span> <span class="nn">sklearn.model_selection</span> <span class="kn">import</span> <span class="n">train_test_split</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>

<span class="n">m</span> <span class="o">=</span> <span class="mi">100</span>
<span class="n">X_train</span> <span class="o">=</span> <span class="mi">6</span> <span class="o">*</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">rand</span><span class="p">(</span><span class="n">m</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span> <span class="o">-</span> <span class="mi">3</span>
<span class="n">y_train</span> <span class="o">=</span> <span class="mf">0.7</span> <span class="o">*</span> <span class="n">X_train</span> <span class="o">**</span><span class="mi">2</span> <span class="o">+</span> <span class="n">X_train</span> <span class="o">+</span> <span class="mi">2</span> <span class="o">+</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="n">m</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span>

<span class="n">plt</span><span class="o">.</span><span class="n">scatter</span><span class="p">(</span><span class="n">X</span><span class="p">,</span> <span class="n">y</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">show</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_png output_subarea ">
<img src="data:image/png;base64,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"
>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#54617;&#49845;">&#54617;&#49845;<a class="anchor-link" href="#&#54617;&#49845;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">poly</span> <span class="o">=</span> <span class="n">PolynomialFeatures</span><span class="p">(</span><span class="n">degree</span><span class="o">=</span><span class="mi">2</span><span class="p">,</span> <span class="n">include_bias</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>

<span class="n">X_train_poly</span> <span class="o">=</span> <span class="n">poly</span><span class="o">.</span><span class="n">fit_transform</span><span class="p">(</span><span class="n">X_train</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.linear_model</span> <span class="kn">import</span> <span class="n">LinearRegression</span>

<span class="n">lin_reg</span> <span class="o">=</span> <span class="n">LinearRegression</span><span class="p">()</span>
<span class="n">lin_reg</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X_train_poly</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>LinearRegression()</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#53580;&#49828;&#53944;-&#45936;&#51060;&#53552;-&#49373;&#49457;">&#53580;&#49828;&#53944; &#45936;&#51060;&#53552; &#49373;&#49457;<a class="anchor-link" href="#&#53580;&#49828;&#53944;-&#45936;&#51060;&#53552;-&#49373;&#49457;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">m</span> <span class="o">=</span> <span class="mi">20</span>
<span class="n">X_test</span> <span class="o">=</span> <span class="mi">6</span> <span class="o">*</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">rand</span><span class="p">(</span><span class="n">m</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span> <span class="o">-</span> <span class="mi">3</span>
<span class="n">y_test</span> <span class="o">=</span> <span class="mf">0.7</span> <span class="o">*</span> <span class="n">X_test</span> <span class="o">**</span><span class="mi">2</span> <span class="o">+</span> <span class="n">X_test</span> <span class="o">+</span> <span class="mi">2</span> <span class="o">+</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randn</span><span class="p">(</span><span class="n">m</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#48320;&#54872;">&#48320;&#54872;<a class="anchor-link" href="#&#48320;&#54872;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X_test_poly</span> <span class="o">=</span> <span class="n">poly</span><span class="o">.</span><span class="n">transform</span><span class="p">(</span><span class="n">X_test</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#50696;&#52769;">&#50696;&#52769;<a class="anchor-link" href="#&#50696;&#52769;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">y_pred</span> <span class="o">=</span> <span class="n">lin_reg</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">X_test_poly</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">lin_reg</span><span class="o">.</span><span class="n">score</span><span class="p">(</span><span class="n">X_test_poly</span><span class="p">,</span> <span class="n">y_test</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>0.7699902117540998</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 


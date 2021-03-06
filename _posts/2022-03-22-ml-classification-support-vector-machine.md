---
keywords: fastai
description: KNU AIR week2
title: "[MachineLearning] Classification - Support Vector Machine"
toc: false
badges: false
comments: false
categories: [classification]
hide_{github,colab,binder,deepnote}_badge: true
nb_path: _notebooks/ml-classification-support-vector-machine.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/ml-classification-support-vector-machine.ipynb
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
<h3 id="SVM(Support-Vector-Machine)&#46976;?">SVM(Support Vector Machine)&#46976;?<a class="anchor-link" href="#SVM(Support-Vector-Machine)&#46976;?"> </a></h3><ul>
<li><p>결정 경계(Decision Boundary), 즉 분류를 위한 기준 선을 정의하는 모델</p>
</li>
<li><p>새로운 데이터가 주어졌을 때, 어느쪽 결정경계에 포함하는지에 따라 분류</p>
</li>
</ul>
<h3 id="&#51339;&#51008;-&#44208;&#51221;&#44221;&#44228;&#46976;?">&#51339;&#51008; &#44208;&#51221;&#44221;&#44228;&#46976;?<a class="anchor-link" href="#&#51339;&#51008;-&#44208;&#51221;&#44221;&#44228;&#46976;?"> </a></h3><ul>
<li><p>데이터 군으로부터 멀리 떨어져있는 결정 경게</p>
</li>
<li><p>서포트 벡터: 결정 경계와 가까이 있는 데이터들</p>
</li>
<li><p>Margin: 결정 경계와 서포트 벡터 사이의 거리</p>
</li>
<li><p>최적의 결정경게는 Margin을 최대화 한다</p>
</li>
<li><p>n개의 속성을 가진 데이터에는 최소 n+1개의 서포트 벡터가 존재</p>
</li>
</ul>
<h3 id="SVM-&#51109;&#51216;">SVM &#51109;&#51216;<a class="anchor-link" href="#SVM-&#51109;&#51216;"> </a></h3><ul>
<li><p>SVM에서 결정 경계는 서포트 벡터에 의해 정의되므로, 데이터 중에서 서포트 벡터만을 잘 선별하면 필요없는 데이터들을 무시할 수 있다.</p>
</li>
<li><p>이로인해 매우 빠르다는 장점이 있다.</p>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.datasets</span> <span class="kn">import</span> <span class="n">load_iris</span>
<span class="kn">from</span> <span class="nn">sklearn.model_selection</span> <span class="kn">import</span> <span class="n">train_test_split</span>

<span class="n">X</span><span class="p">,</span> <span class="n">y</span> <span class="o">=</span> <span class="n">load_iris</span><span class="p">(</span><span class="n">return_X_y</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>

<span class="n">X_train</span><span class="p">,</span> <span class="n">X_valid</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">y_valid</span> <span class="o">=</span> <span class="n">train_test_split</span><span class="p">(</span><span class="n">X</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">stratify</span><span class="o">=</span><span class="n">y</span><span class="p">,</span> <span class="n">test_size</span><span class="o">=</span><span class="mf">0.3</span><span class="p">,</span> <span class="n">random_state</span><span class="o">=</span><span class="mi">34</span><span class="p">)</span>

<span class="n">X_train</span><span class="o">.</span><span class="n">shape</span><span class="p">,</span> <span class="n">y_train</span><span class="o">.</span><span class="n">shape</span><span class="p">,</span> <span class="n">X_valid</span><span class="o">.</span><span class="n">shape</span><span class="p">,</span> <span class="n">y_valid</span><span class="o">.</span><span class="n">shape</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>((105, 4), (105,), (45, 4), (45,))</pre>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.svm</span> <span class="kn">import</span> <span class="n">SVC</span>

<span class="n">classifier</span> <span class="o">=</span> <span class="n">SVC</span><span class="p">(</span><span class="n">kernel</span> <span class="o">=</span> <span class="s1">&#39;linear&#39;</span><span class="p">)</span>


<span class="n">classifier</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span> 
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>SVC(kernel=&#39;linear&#39;)</pre>
</div>

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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">classifier</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">X_valid</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>array([2, 1, 2, 1, 0, 1, 2, 0, 2, 2, 2, 0, 1, 2, 0, 1, 0, 0, 1, 2, 2, 0,
       1, 1, 2, 1, 1, 1, 1, 1, 0, 0, 1, 2, 2, 0, 2, 1, 0, 2, 0, 0, 2, 0,
       0])</pre>
</div>

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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">classifier</span><span class="o">.</span><span class="n">score</span><span class="p">(</span><span class="n">X_valid</span><span class="p">,</span> <span class="n">y_valid</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>0.9555555555555556</pre>
</div>

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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">classifier</span><span class="o">.</span><span class="n">score</span><span class="p">(</span><span class="n">X_valid</span><span class="p">,</span> <span class="n">y_valid</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea ">
<pre>0.9555555555555556</pre>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 


---
keywords: fastai
description: KNU AIR week3
title: "[MachineLearning] Reinforcement learning - Genetic Algorithm"
toc: false
badges: false
comments: false
categories: [reinforcement learining]
hide_{github,colab,binder,deepnote}_badge: true
nb_path: _notebooks/ml-reinforcement-learning-genetic-algorithm.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/ml-reinforcement-learning-genetic-algorithm.ipynb
-->

<div class="container" id="notebook-container">
        
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p><strong>Content creators:</strong> 조동현</p>
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
<h2 id="Genetic-Algorithm&#51060;&#46976;?">Genetic Algorithm&#51060;&#46976;?<a class="anchor-link" href="#Genetic-Algorithm&#51060;&#46976;?"> </a></h2><ul>
<li>"유전자 알고리즘"을 뜻함</li>
<li>자연계의 진화 연산을 컴퓨팅의 최적화 분야에 적용한 것</li>
</ul>
<h2 id="Step">Step<a class="anchor-link" href="#Step"> </a></h2><blockquote><h3 id="1.-&#51665;&#45800;-&#52488;&#44592;&#54868;">1. &#51665;&#45800; &#52488;&#44592;&#54868;<a class="anchor-link" href="#1.-&#51665;&#45800;-&#52488;&#44592;&#54868;"> </a></h3>
<pre><code>문제를 정의하고, 문제를 염색체 형태로 표현한 후 N개의 집단으로 이루어진 초기 염색체 집단을 생성</code></pre>
<h3 id="2.-&#51201;&#54633;&#46020;-&#44228;&#49328;">2. &#51201;&#54633;&#46020; &#44228;&#49328;<a class="anchor-link" href="#2.-&#51201;&#54633;&#46020;-&#44228;&#49328;"> </a></h3>
<pre><code>염색체의 적합도를 측정하는 적합도 함수를 정의하고 계산</code></pre>
<h3 id="3.-&#51333;&#47308;-&#51312;&#44148;-&#54869;&#51064;">3. &#51333;&#47308; &#51312;&#44148; &#54869;&#51064;<a class="anchor-link" href="#3.-&#51333;&#47308;-&#51312;&#44148;-&#54869;&#51064;"> </a></h3>
<pre><code>도출된 적합도가 종료 조건을 만족하면 알고리즘을 종료하고, 만족하지 못하면 다음 단계로 넘어감</code></pre>
<h3 id="4.-&#49440;&#53469;">4. &#49440;&#53469;<a class="anchor-link" href="#4.-&#49440;&#53469;"> </a></h3>
<pre><code>현재의 해당 집단에서 부모 염색체를 한 쌍 선택함. 단, 적합도가 높은 염색체가 선택될 확률이 높아야 함</code></pre>
<h3 id="5.-&#44368;&#52264;">5. &#44368;&#52264;<a class="anchor-link" href="#5.-&#44368;&#52264;"> </a></h3>
<pre><code>부모 염색체의 일부를 교차시켜서 자식 염색체를 한 쌍 생성함</code></pre>
<h3 id="6.-&#46028;&#50672;&#48320;&#51060;">6. &#46028;&#50672;&#48320;&#51060;<a class="anchor-link" href="#6.-&#46028;&#50672;&#48320;&#51060;"> </a></h3>
<pre><code>만들어진 자식 염색체의 일부를 랜덤하게 선택하여 변경함
부모 염색체와 동일한 수의 자식 염색체가 생성되었으면 이것으로 모두 부모 염색체를 교체하고 다시 2번으로 돌아감</code></pre>
</blockquote>
<hr>
<h2 id="&#50672;&#49328;&#51088;">&#50672;&#49328;&#51088;<a class="anchor-link" href="#&#50672;&#49328;&#51088;"> </a></h2><blockquote><h3 id="&#49440;&#53469;-&#50672;&#49328;&#51088;-(select)">&#49440;&#53469; &#50672;&#49328;&#51088; (select)<a class="anchor-link" href="#&#49440;&#53469;-&#50672;&#49328;&#51088;-(select)"> </a></h3>
<pre><code> 선택 연산자란 좋은 적합도 점수를 가진 염색체에게 우선 순위를 부여하고 좋은 유전자를 다음 세대에 전달할 수 있도록 하는 연산자

 보통 룰렛 휠 선택 알고리즘(roulette wheel selection)이 많이 쓰이며 염색체 후보들이 차지하는 룰렛의 비율이 적합도 함수 값에 비례하도록 한 알고리즘임</code></pre>
<h3 id="&#44368;&#52264;-&#50672;&#49328;&#51088;-(crossover)">&#44368;&#52264; &#50672;&#49328;&#51088; (crossover)<a class="anchor-link" href="#&#44368;&#52264;-&#50672;&#49328;&#51088;-(crossover)"> </a></h3>
<pre><code> 교차 연산자는 염색체 간의 교배를 나타내며, 선택 사용자를 사용하여 두 개의 염색체를 선택하고 교차 위치를 임의로 선택함. 그 후 교차 지점을 중심으로 유전자를 서로 교환하여 새로운 자식을 생성</code></pre>
<h3 id="&#46028;&#50672;&#48320;&#51060;-&#50672;&#49328;&#51088;-(mutate)">&#46028;&#50672;&#48320;&#51060; &#50672;&#49328;&#51088; (mutate)<a class="anchor-link" href="#&#46028;&#50672;&#48320;&#51060;-&#50672;&#49328;&#51088;-(mutate)"> </a></h3>
<pre><code>Local minimum을 피하고 개체군의 다양성을 유지하기 위한 연산자로써, 자손에 무작위 유전자를 삽입(or 변경)하는 연산자임.</code></pre>
</blockquote>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Pseudo-Code">Pseudo Code<a class="anchor-link" href="#Pseudo-Code"> </a></h2>
<pre><code>Genetic Algorithm(population, FitnessFunc)
{
  repeat
      new_population = []
      for i = 1 to size(population) do
          father = select(population, FitnessFunc)
          mother = select(population, FitnessFunc)
          child = crossover(father, mother)
          if (난수 &lt; 변이 확률) then child = mutate(child)
          new_population.append(child)
      population = new_population
  until 종료 조건 만족할 때 까지

  return 가장 적합한 개체
}</code></pre>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<hr>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="2.Example">2.Example<a class="anchor-link" href="#2.Example"> </a></h1><ul>
<li>고전적인 예제인 0 ~ 31 범위 안에서 x^2의 값을 최대화 하는 x 값을 유전자 알고리즘을 이용해 찾아내보자</li>
</ul>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">random</span>

<span class="n">POPULATION_SIZE</span> <span class="o">=</span> <span class="mi">4</span>
<span class="n">MUTATION_RATE</span> <span class="o">=</span> <span class="mf">0.1</span>
<span class="n">SIZE</span> <span class="o">=</span> <span class="mi">5</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#50684;&#49353;&#52404;-&#53364;&#47000;&#49828;-&#44396;&#54788;">&#50684;&#49353;&#52404; &#53364;&#47000;&#49828; &#44396;&#54788;<a class="anchor-link" href="#&#50684;&#49353;&#52404;-&#53364;&#47000;&#49828;-&#44396;&#54788;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">class</span> <span class="nc">Chromosome</span><span class="p">:</span>
    <span class="k">def</span> <span class="fm">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">g</span><span class="o">=</span><span class="p">[]):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">genes</span> <span class="o">=</span> <span class="n">g</span><span class="o">.</span><span class="n">copy</span><span class="p">()</span> <span class="c1"># 유전자는 리스트로 구현 </span>
        <span class="bp">self</span><span class="o">.</span><span class="n">fitness</span> <span class="o">=</span> <span class="mi">0</span> <span class="c1"># 적합도</span>
        <span class="k">if</span> <span class="bp">self</span><span class="o">.</span><span class="n">genes</span><span class="o">.</span><span class="fm">__len__</span><span class="p">()</span><span class="o">==</span><span class="mi">0</span><span class="p">:</span> <span class="c1"># 염색체가 초기 상태이면 초기화 </span>
            <span class="n">i</span> <span class="o">=</span> <span class="mi">0</span>
            <span class="k">while</span> <span class="n">i</span><span class="o">&lt;</span><span class="n">SIZE</span><span class="p">:</span>
                <span class="k">if</span> <span class="n">random</span><span class="o">.</span><span class="n">random</span><span class="p">()</span> <span class="o">&gt;=</span> <span class="mf">0.5</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">genes</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="mi">1</span><span class="p">)</span>
                <span class="k">else</span><span class="p">:</span> <span class="bp">self</span><span class="o">.</span><span class="n">genes</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="mi">0</span><span class="p">)</span>
                <span class="n">i</span> <span class="o">+=</span> <span class="mi">1</span>
    
    <span class="k">def</span> <span class="nf">cal_fitness</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span> <span class="c1"># 적합도를 계산 </span>
        <span class="bp">self</span><span class="o">.</span><span class="n">fitness</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="n">value</span> <span class="o">=</span> <span class="mi">0</span>
        <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">SIZE</span><span class="p">):</span>
            <span class="n">value</span> <span class="o">+=</span> <span class="bp">self</span><span class="o">.</span><span class="n">genes</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">*</span> <span class="nb">pow</span><span class="p">(</span><span class="mi">2</span><span class="p">,</span> <span class="n">SIZE</span><span class="o">-</span><span class="mi">1</span><span class="o">-</span><span class="n">i</span><span class="p">)</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">fitness</span> <span class="o">=</span> <span class="n">value</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">fitness</span>

    <span class="k">def</span> <span class="fm">__str__</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">genes</span><span class="o">.</span><span class="fm">__str__</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#50684;&#49353;&#52404;&#50752;-&#51201;&#54633;&#46020;-&#52636;&#47141;-&#54632;&#49688;">&#50684;&#49353;&#52404;&#50752; &#51201;&#54633;&#46020; &#52636;&#47141; &#54632;&#49688;<a class="anchor-link" href="#&#50684;&#49353;&#52404;&#50752;-&#51201;&#54633;&#46020;-&#52636;&#47141;-&#54632;&#49688;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">print_p</span><span class="p">(</span><span class="n">pop</span><span class="p">):</span>
    <span class="n">i</span> <span class="o">=</span> <span class="mi">0</span>
    <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="n">pop</span><span class="p">:</span>
        <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;염색체 #&quot;</span><span class="p">,</span> <span class="n">i</span><span class="p">,</span> <span class="s2">&quot;=&quot;</span><span class="p">,</span> <span class="n">x</span><span class="p">,</span> <span class="s2">&quot;적합도=&quot;</span><span class="p">,</span> <span class="n">x</span><span class="o">.</span><span class="n">cal_fitness</span><span class="p">())</span>
        <span class="n">i</span> <span class="o">+=</span> <span class="mi">1</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#49440;&#53469;-&#50672;&#49328;">&#49440;&#53469; &#50672;&#49328;<a class="anchor-link" href="#&#49440;&#53469;-&#50672;&#49328;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">select</span><span class="p">(</span><span class="n">pop</span><span class="p">):</span>
    <span class="n">max_value</span>  <span class="o">=</span> <span class="nb">sum</span><span class="p">([</span><span class="n">c</span><span class="o">.</span><span class="n">cal_fitness</span><span class="p">()</span> <span class="k">for</span> <span class="n">c</span> <span class="ow">in</span> <span class="n">population</span><span class="p">])</span>
    <span class="n">pick</span>    <span class="o">=</span> <span class="n">random</span><span class="o">.</span><span class="n">uniform</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="n">max_value</span><span class="p">)</span>
    <span class="n">current</span> <span class="o">=</span> <span class="mi">0</span>
    
    <span class="k">for</span> <span class="n">c</span> <span class="ow">in</span> <span class="n">pop</span><span class="p">:</span>
        <span class="n">current</span> <span class="o">+=</span> <span class="n">c</span><span class="o">.</span><span class="n">cal_fitness</span><span class="p">()</span>
        <span class="k">if</span> <span class="n">current</span> <span class="o">&gt;</span> <span class="n">pick</span><span class="p">:</span>
            <span class="k">return</span> <span class="n">c</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#44368;&#52264;-&#50672;&#49328;">&#44368;&#52264; &#50672;&#49328;<a class="anchor-link" href="#&#44368;&#52264;-&#50672;&#49328;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">crossover</span><span class="p">(</span><span class="n">pop</span><span class="p">):</span>
    <span class="n">father</span> <span class="o">=</span> <span class="n">select</span><span class="p">(</span><span class="n">pop</span><span class="p">)</span>
    <span class="n">mother</span> <span class="o">=</span> <span class="n">select</span><span class="p">(</span><span class="n">pop</span><span class="p">)</span>
    <span class="n">index</span> <span class="o">=</span> <span class="n">random</span><span class="o">.</span><span class="n">randint</span><span class="p">(</span><span class="mi">1</span><span class="p">,</span> <span class="n">SIZE</span> <span class="o">-</span> <span class="mi">2</span><span class="p">)</span>
    <span class="n">child1</span> <span class="o">=</span> <span class="n">father</span><span class="o">.</span><span class="n">genes</span><span class="p">[:</span><span class="n">index</span><span class="p">]</span> <span class="o">+</span> <span class="n">mother</span><span class="o">.</span><span class="n">genes</span><span class="p">[</span><span class="n">index</span><span class="p">:]</span> 
    <span class="n">child2</span> <span class="o">=</span> <span class="n">mother</span><span class="o">.</span><span class="n">genes</span><span class="p">[:</span><span class="n">index</span><span class="p">]</span> <span class="o">+</span> <span class="n">father</span><span class="o">.</span><span class="n">genes</span><span class="p">[</span><span class="n">index</span><span class="p">:]</span> 
    <span class="k">return</span> <span class="p">(</span><span class="n">child1</span><span class="p">,</span> <span class="n">child2</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#46028;&#50672;&#48320;&#51060;-&#50672;&#49328;">&#46028;&#50672;&#48320;&#51060; &#50672;&#49328;<a class="anchor-link" href="#&#46028;&#50672;&#48320;&#51060;-&#50672;&#49328;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">mutate</span><span class="p">(</span><span class="n">c</span><span class="p">):</span>
    <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">SIZE</span><span class="p">):</span>
        <span class="k">if</span> <span class="n">random</span><span class="o">.</span><span class="n">random</span><span class="p">()</span> <span class="o">&lt;</span> <span class="n">MUTATION_RATE</span><span class="p">:</span>
            <span class="k">if</span> <span class="n">random</span><span class="o">.</span><span class="n">random</span><span class="p">()</span> <span class="o">&lt;</span> <span class="mf">0.5</span><span class="p">:</span>
                <span class="n">c</span><span class="o">.</span><span class="n">genes</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">=</span> <span class="mi">1</span>
            <span class="k">else</span><span class="p">:</span>
                <span class="n">c</span><span class="o">.</span><span class="n">genes</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">=</span> <span class="mi">0</span>
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="&#47700;&#51064;-&#54532;&#47196;&#44536;&#47016;">&#47700;&#51064; &#54532;&#47196;&#44536;&#47016;<a class="anchor-link" href="#&#47700;&#51064;-&#54532;&#47196;&#44536;&#47016;"> </a></h3>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">population</span> <span class="o">=</span> <span class="p">[]</span>
<span class="n">i</span><span class="o">=</span><span class="mi">0</span>

<span class="c1"># 초기 염색체를 생성하여 객체 집단에 추가</span>
<span class="k">while</span> <span class="n">i</span><span class="o">&lt;</span><span class="n">POPULATION_SIZE</span><span class="p">:</span>
    <span class="n">population</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">Chromosome</span><span class="p">())</span>
    <span class="n">i</span> <span class="o">+=</span> <span class="mi">1</span>
    
<span class="n">count</span><span class="o">=</span><span class="mi">0</span>
<span class="n">population</span><span class="o">.</span><span class="n">sort</span><span class="p">(</span><span class="n">key</span><span class="o">=</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">x</span><span class="o">.</span><span class="n">cal_fitness</span><span class="p">(),</span> <span class="n">reverse</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;세대 번호=&quot;</span><span class="p">,</span> <span class="n">count</span><span class="p">)</span>
<span class="n">print_p</span><span class="p">(</span><span class="n">population</span><span class="p">)</span>
<span class="n">count</span><span class="o">=</span><span class="mi">1</span>

<span class="k">while</span> <span class="n">population</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span><span class="o">.</span><span class="n">cal_fitness</span><span class="p">()</span> <span class="o">&lt;</span> <span class="mi">31</span><span class="p">:</span>
    <span class="n">new_pop</span> <span class="o">=</span> <span class="p">[]</span>

    <span class="c1"># 선택과 교차 연산</span>
    <span class="k">for</span> <span class="n">_</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">POPULATION_SIZE</span><span class="o">//</span><span class="mi">2</span><span class="p">):</span>
        <span class="n">c1</span><span class="p">,</span> <span class="n">c2</span> <span class="o">=</span> <span class="n">crossover</span><span class="p">(</span><span class="n">population</span><span class="p">);</span>
        <span class="n">new_pop</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">Chromosome</span><span class="p">(</span><span class="n">c1</span><span class="p">));</span>
        <span class="n">new_pop</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">Chromosome</span><span class="p">(</span><span class="n">c2</span><span class="p">));</span>

    <span class="c1"># 자식 세대가 부모 세대를 대체</span>
    <span class="c1"># 깊은 복사를 수행</span>
    <span class="n">population</span> <span class="o">=</span> <span class="n">new_pop</span><span class="o">.</span><span class="n">copy</span><span class="p">();</span>    
    
    <span class="c1"># 돌연변이 연산</span>
    <span class="k">for</span> <span class="n">c</span> <span class="ow">in</span> <span class="n">population</span><span class="p">:</span> <span class="n">mutate</span><span class="p">(</span><span class="n">c</span><span class="p">)</span>

    <span class="c1"># 출력을 위한 정렬</span>
    <span class="n">population</span><span class="o">.</span><span class="n">sort</span><span class="p">(</span><span class="n">key</span><span class="o">=</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="n">x</span><span class="o">.</span><span class="n">cal_fitness</span><span class="p">(),</span> <span class="n">reverse</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
    <span class="nb">print</span><span class="p">(</span><span class="s2">&quot;세대 번호=&quot;</span><span class="p">,</span> <span class="n">count</span><span class="p">)</span>
    <span class="n">print_p</span><span class="p">(</span><span class="n">population</span><span class="p">)</span>
    <span class="n">count</span> <span class="o">+=</span> <span class="mi">1</span>
    <span class="k">if</span> <span class="n">count</span> <span class="o">&gt;</span> <span class="mi">100</span> <span class="p">:</span> <span class="k">break</span><span class="p">;</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>세대 번호= 0
염색체 # 0 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 1 = [0, 1, 1, 0, 1] 적합도= 13
염색체 # 2 = [0, 0, 1, 0, 0] 적합도= 4
염색체 # 3 = [0, 0, 0, 1, 1] 적합도= 3

세대 번호= 1
염색체 # 0 = [1, 1, 1, 0, 1] 적합도= 29
염색체 # 1 = [1, 0, 0, 0, 1] 적합도= 17
염색체 # 2 = [0, 1, 1, 1, 0] 적합도= 14
염색체 # 3 = [0, 0, 0, 1, 0] 적합도= 2

세대 번호= 2
염색체 # 0 = [1, 1, 1, 0, 1] 적합도= 29
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 0, 1] 적합도= 17
염색체 # 3 = [0, 0, 0, 0, 1] 적합도= 1

세대 번호= 3
염색체 # 0 = [1, 1, 1, 0, 1] 적합도= 29
염색체 # 1 = [1, 1, 1, 0, 1] 적합도= 29
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 0, 1] 적합도= 17

세대 번호= 4
염색체 # 0 = [1, 1, 1, 0, 1] 적합도= 29
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 5
염색체 # 0 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 6
염색체 # 0 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 7
염색체 # 0 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 8
염색체 # 0 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 9
염색체 # 0 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [0, 0, 0, 1, 0] 적합도= 2

세대 번호= 10
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 11
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 12
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 13
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [0, 0, 0, 1, 0] 적합도= 2

세대 번호= 14
염색체 # 0 = [1, 1, 0, 1, 1] 적합도= 27
염색체 # 1 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 2 = [1, 1, 0, 0, 0] 적합도= 24
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 15
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 0, 0] 적합도= 16
염색체 # 3 = [0, 1, 0, 1, 0] 적합도= 10

세대 번호= 16
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 1, 0, 0, 0] 적합도= 24
염색체 # 2 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 3 = [0, 0, 0, 1, 0] 적합도= 2

세대 번호= 17
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 2 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 3 = [1, 0, 1, 1, 0] 적합도= 22

세대 번호= 18
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 2 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 19
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 2 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 20
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 21
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 0, 1, 0, 0] 적합도= 20
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 22
염색체 # 0 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 1 = [1, 0, 1, 0, 1] 적합도= 21
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [0, 0, 0, 1, 0] 적합도= 2

세대 번호= 23
염색체 # 0 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 1 = [1, 0, 1, 0, 1] 적합도= 21
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 1, 0] 적합도= 18

세대 번호= 24
염색체 # 0 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 0, 0] 적합도= 16

세대 번호= 25
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 2 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 3 = [1, 0, 0, 0, 0] 적합도= 16

세대 번호= 26
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 0, 0, 0] 적합도= 24
염색체 # 2 = [1, 0, 1, 1, 0] 적합도= 22
염색체 # 3 = [1, 0, 0, 1, 1] 적합도= 19

세대 번호= 27
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 2 = [1, 1, 0, 0, 0] 적합도= 24
염색체 # 3 = [1, 0, 0, 0, 0] 적합도= 16

세대 번호= 28
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [1, 0, 0, 0, 0] 적합도= 16
염색체 # 2 = [1, 0, 0, 0, 0] 적합도= 16
염색체 # 3 = [0, 1, 0, 0, 0] 적합도= 8

세대 번호= 29
염색체 # 0 = [1, 0, 0, 1, 0] 적합도= 18
염색체 # 1 = [0, 1, 0, 1, 0] 적합도= 10
염색체 # 2 = [0, 1, 0, 0, 0] 적합도= 8
염색체 # 3 = [0, 1, 0, 0, 0] 적합도= 8

세대 번호= 30
염색체 # 0 = [0, 1, 0, 1, 0] 적합도= 10
염색체 # 1 = [0, 1, 0, 1, 0] 적합도= 10
염색체 # 2 = [0, 1, 0, 1, 0] 적합도= 10
염색체 # 3 = [0, 1, 0, 0, 0] 적합도= 8

세대 번호= 31
염색체 # 0 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 1 = [0, 1, 0, 1, 0] 적합도= 10
염색체 # 2 = [0, 1, 0, 1, 0] 적합도= 10
염색체 # 3 = [0, 1, 0, 0, 0] 적합도= 8

세대 번호= 32
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 0, 1, 0] 적합도= 26
염색체 # 2 = [1, 1, 0, 0, 0] 적합도= 24
염색체 # 3 = [0, 1, 0, 1, 0] 적합도= 10

세대 번호= 33
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 2 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 3 = [1, 1, 0, 1, 0] 적합도= 26

세대 번호= 34
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 2 = [1, 1, 0, 0, 0] 적합도= 24
염색체 # 3 = [0, 1, 1, 1, 0] 적합도= 14

세대 번호= 35
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 2 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 3 = [0, 1, 1, 1, 0] 적합도= 14

세대 번호= 36
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 2 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 3 = [1, 1, 1, 1, 0] 적합도= 30

세대 번호= 37
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 2 = [0, 1, 1, 1, 0] 적합도= 14
염색체 # 3 = [0, 1, 1, 0, 0] 적합도= 12

세대 번호= 38
염색체 # 0 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 1 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 2 = [0, 1, 1, 1, 0] 적합도= 14
염색체 # 3 = [0, 1, 1, 1, 0] 적합도= 14

세대 번호= 39
염색체 # 0 = [1, 1, 1, 1, 1] 적합도= 31
염색체 # 1 = [1, 1, 1, 1, 0] 적합도= 30
염색체 # 2 = [0, 1, 1, 1, 1] 적합도= 15
염색체 # 3 = [0, 1, 1, 1, 0] 적합도= 14

</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 


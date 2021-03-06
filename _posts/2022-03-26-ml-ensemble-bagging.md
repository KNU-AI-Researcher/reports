---
keywords: fastai
description: KNU AIR week4
title: "[MachineLearning] Ensemble Learning - Bagging"
toc: false
badges: false
comments: false
categories: [ensemble learning]
hide_{github,colab,binder,deepnote}_badge: true
nb_path: _notebooks/ml-ensemble-bagging.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/ml-ensemble-bagging.ipynb
-->

<div class="container" id="notebook-container">
        
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p><strong>Content creators:</strong> 이주형, 이중원</p>
<p><strong>Content reviewers:</strong></p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="1.-Overview">1. Overview<a class="anchor-link" href="#1.-Overview"> </a></h1><h3 id="Bagging---Bootstrap-Aggregating">Bagging - Bootstrap Aggregating<a class="anchor-link" href="#Bagging---Bootstrap-Aggregating"> </a></h3><ul>
<li>Each member of the ensemble is constructed from a <strong>different training dataset</strong>.</li>
<li>Each dataset is generated by sampling from the total N data examples, choosing N items uniformly at random with replacement.</li>
<li>Probability that an instance is not included in a bootstrap(→<em>Out of Bag, OOB</em>): \
$p=\left( 1-\frac{1}{N}\right)^N \rightarrow \lim_{N \to \infty} \left( 1-\frac{1}{N}\right)^N = e^{-1} \approx 0.368$</li>
</ul>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Result-Aggregating-for-Classification-task">Result Aggregating for Classification task<a class="anchor-link" href="#Result-Aggregating-for-Classification-task"> </a></h3><ul>
<li>Voting Strategy<ul>
<li>Majority Voting</li>
<li>Weighting Voting, and the weight could be <ul>
<li>validation acc. of individual models</li>
<li>predicted prob. for each class</li>
<li>can use both for weight</li>
</ul>
</li>
</ul>
</li>
<li>Stacking : Result aggregation<ul>
<li>Use another prediction model, i.e. <em>meta-classifier</em>, to aggregate the results</li>
<li>Source : Predictions made by ensemble members</li>
<li>Target : Actual true label</li>
</ul>
</li>
</ul>
<p><a href="https://github.com/pilsung-kang/Business-Analytics-IME654-">Reference</a></p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Bagging-:-Examples">Bagging : Examples<a class="anchor-link" href="#Bagging-:-Examples"> </a></h2>
</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">matplotlib</span> inline

<span class="kn">from</span> <span class="nn">sklearn.ensemble</span> <span class="kn">import</span> <span class="n">BaggingRegressor</span>
<span class="kn">from</span> <span class="nn">sklearn.datasets</span> <span class="kn">import</span> <span class="n">fetch_california_housing</span>
<span class="kn">from</span> <span class="nn">sklearn.neighbors</span> <span class="kn">import</span> <span class="n">KNeighborsRegressor</span> <span class="k">as</span> <span class="n">NN</span>
<span class="kn">from</span> <span class="nn">sklearn.metrics</span> <span class="kn">import</span> <span class="n">r2_score</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>

<span class="kn">from</span> <span class="nn">matplotlib</span> <span class="kn">import</span> <span class="n">pyplot</span> <span class="k">as</span> <span class="n">plt</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">SEED</span> <span class="o">=</span> <span class="mi">42</span>
<span class="n">X</span><span class="p">,</span> <span class="n">y</span> <span class="o">=</span> <span class="n">fetch_california_housing</span><span class="p">(</span><span class="n">data_home</span><span class="o">=</span><span class="s1">&#39;./&#39;</span><span class="p">,</span> <span class="n">return_X_y</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">clf</span> <span class="o">=</span> <span class="n">BaggingRegressor</span><span class="p">(</span> <span class="c1"># experiment group</span>
                    <span class="n">base_estimator</span><span class="o">=</span><span class="n">NN</span><span class="p">(),</span>
                    <span class="n">n_estimators</span><span class="o">=</span><span class="mi">10</span><span class="p">,</span>
                    <span class="n">max_samples</span><span class="o">=</span><span class="mf">0.3</span><span class="p">,</span>
                    <span class="n">oob_score</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span>
                    <span class="n">random_state</span><span class="o">=</span><span class="n">SEED</span>
                <span class="p">)</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X</span><span class="p">,</span> <span class="n">y</span><span class="p">)</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">clf</span><span class="o">.</span><span class="n">oob_score_</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>0.1475162707581028</pre>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X_train</span><span class="p">,</span> <span class="n">y_train</span> <span class="o">=</span> <span class="n">X</span><span class="p">[</span><span class="n">np</span><span class="o">.</span><span class="n">unique</span><span class="p">(</span><span class="n">clf</span><span class="o">.</span><span class="n">estimators_samples_</span><span class="p">),:],</span> <span class="n">y</span><span class="p">[</span><span class="n">np</span><span class="o">.</span><span class="n">unique</span><span class="p">(</span><span class="n">clf</span><span class="o">.</span><span class="n">estimators_samples_</span><span class="p">)]</span>
<span class="n">X_test</span><span class="p">,</span> <span class="n">y_test</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">delete</span><span class="p">(</span><span class="n">X</span><span class="p">,</span> <span class="n">np</span><span class="o">.</span><span class="n">unique</span><span class="p">(</span><span class="n">clf</span><span class="o">.</span><span class="n">estimators_samples_</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">),</span> <span class="n">np</span><span class="o">.</span><span class="n">delete</span><span class="p">(</span><span class="n">y</span><span class="p">,</span> <span class="n">np</span><span class="o">.</span><span class="n">unique</span><span class="p">(</span><span class="n">clf</span><span class="o">.</span><span class="n">estimators_samples_</span><span class="p">))</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">component_res</span> <span class="o">=</span> <span class="p">[]</span>
<span class="k">for</span> <span class="n">estimator</span> <span class="ow">in</span> <span class="n">clf</span><span class="o">.</span><span class="n">estimators_</span><span class="p">:</span>
    <span class="n">component_pred</span> <span class="o">=</span> <span class="n">estimator</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">X_test</span><span class="p">)</span>
    <span class="n">component_res</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">r2_score</span><span class="p">(</span><span class="n">y_test</span><span class="p">,</span> <span class="n">component_pred</span><span class="p">))</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">component_res</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_text output_subarea output_execute_result">
<pre>[-0.03174907758299472,
 -0.021045219553941896,
 0.003535488064889125,
 -0.03675586372593598,
 -0.005254866062958996,
 0.02225931720479135,
 -0.00932398344070906,
 -0.02436691937209745,
 -0.005065799458619846,
 -0.02227930930171418]</pre>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">nn_clf</span> <span class="o">=</span> <span class="n">NN</span><span class="p">()</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>
<span class="n">pred</span> <span class="o">=</span> <span class="n">nn_clf</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">X_test</span><span class="p">)</span>
<span class="n">nn_clf_score</span> <span class="o">=</span> <span class="n">r2_score</span><span class="p">(</span><span class="n">y_test</span><span class="p">,</span> <span class="n">pred</span><span class="p">)</span>
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
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">plt</span><span class="o">.</span><span class="n">bar</span><span class="p">([</span><span class="sa">r</span><span class="s1">&#39;$E$&#39;</span><span class="p">],</span> <span class="n">clf</span><span class="o">.</span><span class="n">oob_score_</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">&#39;bagging(ensembled)&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">bar</span><span class="p">([</span><span class="sa">f</span><span class="s1">&#39;c</span><span class="si">{</span><span class="n">i</span><span class="si">}</span><span class="s1">&#39;</span> <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">10</span><span class="p">)],</span> <span class="n">component_res</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">&#39;bagging(component)&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">bar</span><span class="p">([</span><span class="sa">r</span><span class="s1">&#39;$S$&#39;</span><span class="p">],</span> <span class="n">nn_clf_score</span><span class="p">,</span> <span class="n">label</span><span class="o">=</span><span class="s1">&#39;single model&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">legend</span><span class="p">()</span>
<span class="n">plt</span><span class="o">.</span><span class="n">grid</span><span class="p">(</span><span class="n">axis</span><span class="o">=</span><span class="s1">&#39;y&#39;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">



<div class="output_png output_subarea ">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAYkAAAD6CAYAAABUHLtmAAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjUuMSwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy/YYfK9AAAACXBIWXMAAAsTAAALEwEAmpwYAAAmyklEQVR4nO3deXhV1bnH8e/LJCpjQakVKniLSgYIJQwtBVJQoNWKA1YsKBGRFgRb6+VKH7moiEp7vdVexQEnsBVB4VG5SksdSCutSgIGQkRqoFGD3FYmyyBI4L1/nE16CNkhydmHDP19nuc87HG9a+8cznvWWuesY+6OiIhIRRrVdgVERKTuUpIQEZFQShIiIhJKSUJEREIpSYiISKgmtV2BKLVv3947d+5c29UQEalXVq9evc3dT6toX4NKEp07dyYvL6+2qyEiUq+Y2Ydh+9TdJCIioZQkREQkVCRJwsyGm9lGMysys2kV7B9oZmvMrNTMRpbbd8jM8oPH0rjtXczsnaDMRWbWLIq6iohI1SU8JmFmjYE5wAVACZBrZkvd/b24wz4CsoF/r6CIz909o4LtPwfuc/eFZvYIcB3wcKL1leQ6ePAgJSUl7N+/v7arIvVA8+bN6dixI02bNq3tqkiIKAau+wBF7r4ZwMwWAiOAsiTh7sXBvsNVKdDMDBgM/CDYNB+4HSWJOq+kpISWLVvSuXNnYn9GkYq5O9u3b6ekpIQuXbrUdnUkRBTdTWcCH8etlwTbqqq5meWZ2dtmdkmwrR2wy91La1im1JL9+/fTrl07JQg5LjOjXbt2anXWcXXhI7BnufsWMzsbeMPMCoDPqnqymU0AJgB06NCBnJyc5NRSqqR169bs2bOntqsh9cj+/fv1/7YOiyJJbAE6xa13DLZVibtvCf7dbGY5QE9gCdDGzJoErYnQMt19LjAXIDMz07OysmpwCRKVDRs20LJly9quhtQjzZs3p2fPnrVdDQkRRZLIBbqaWRdiL+Sj+OdYQqXMrC2wz90PmFl7oD/wC3d3M1sBjAQWAmOBlyKoa6jO016JvMzi2RdGXmZ9E/V9rco9LS4u5qKLLmL9+vWRxo733e9+lwULFtCmTZsanf/uu+/y4IMP8sQTT0RbsQS0aNGiwlZgdnY2F110ESNHjqzgrGPF3/+CggL++7//m3nz5kVcWzlREh6TCN7pTwaWAxuA59y90MxmmtnFAGbW28xKgCuAR82sMDi9G5BnZmuBFcDsuE9F3QL81MyKiI1R1J3/TfIvb9myZTVOEAB33303N954Y3QVqqPS09MpKSnho48+qu2qSA1F8j0Jd1/m7ue4+7+5+13BthnuvjRYznX3ju5+qru3c/fUYPuf3T3d3XsE/z4RV+Zmd+/j7l9z9yvc/UAUdZV/DaWlpYwePZpu3boxcuRI9u3bx8yZM+nduzdpaWlMmDCBI7/KmJubS/fu3cnIyGDq1KmkpaUBsG/fPr7//e+TkpLCpZdeSt++fcumfencuTPbtm2juLiYbt26cf3115OamsrQoUP5/PPPKy139+7drFu3jh49egCwd+9exo0bR58+fejZsycvvRRrNM+bN4/LLruM4cOH07VrV/7jP/4DgEOHDpGdnU1aWhrp6encd999AGzatInhw4fTq1cvBgwYwPvvvw/EWgITJ06kX79+nH322eTk5DBu3Di6detGdnb2UfftpptuIjU1lSFDhvDpp58ec19Xr17NoEGD6NWrF8OGDWPr1q1l23v06EGPHj2YM2fOUed873vfY+HChQn8NaU26RvX0iBt3LiRSZMmsWHDBlq1asVDDz3E5MmTyc3NZf369Xz++ee8/PLLAFx77bU8+uij5Ofn07hx47IyHnroIdq2bct7773HnXfeyerVqyuM9cEHH3DDDTdQWFhImzZtWLJkSaXl5uXllSUMgLvuuovBgwezatUqVqxYwdSpU9m7dy8A+fn5LFq0iIKCAhYtWsTHH39Mfn4+W7ZsKevOufbaawGYMGECDzzwAKtXr+bee+9l0qRJZTF27tzJW2+9xX333cfFF1/MTTfdRGFhIQUFBeTn5wOxZJWZmUlhYSGDBg3ijjvuOOo6Dx48yJQpU1i8eDGrV69m3Lhx3HrrrWXX+sADD7B27dpj7k9mZiZvvvlm1f5wUucoSUiD1KlTJ/r37w/AmDFjWLlyJStWrKBv376kp6fzxhtvUFhYyK5du9i9ezff+MY3APjBD/45nLZy5UpGjRoFQFpaGt27d68wVpcuXcjIyACgV69eFBcXV1ru1q1bOe20f064+fvf/57Zs2eTkZFBVlYW+/fvL+ueGTJkCK1bt6Z58+akpKTw4YcfcvbZZ7N582amTJnC7373O1q1asWePXv485//zBVXXEFGRgY//OEP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"
>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 


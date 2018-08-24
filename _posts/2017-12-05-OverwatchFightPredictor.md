---
layout: post
title: "Overwatch Fight Prediction"
date: 2017-12-05
excerpt: "Final part of a Data Science project to predict the winners of fights in Overwatch"
tag:
- Overwatch
- Data Science
- Keras
- Python
- Numpy
- Pandas
project: true
comments: false
---
<center><a href="https://github.com/CVaranese/Data301/tree/master/project">Source Code</a></center>


<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Model">Model</h1>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[1]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="kn">from</span> <span class="nn">sklearn.model_selection</span> <span class="k">import</span> <span class="n">train_test_split</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[2]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="s1">&#39;/data/cvaranese/OverwatchFightsData.csv&#39;</span><span class="p">,</span> <span class="n">index_col</span><span class="o">=</span><span class="mi">0</span><span class="p">)</span>
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt output_prompt">Out[2]:</div>



<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Map</th>
      <th>Roundtype</th>
      <th>Blue_Team</th>
      <th>Red_Team</th>
      <th>time</th>
      <th>len</th>
      <th>KB</th>
      <th>KR</th>
      <th>UB</th>
      <th>UR</th>
      <th>...</th>
      <th>Red_FLA</th>
      <th>Red_GLA</th>
      <th>Red_HOU</th>
      <th>Red_LDN</th>
      <th>Red_NYE</th>
      <th>Red_PHI</th>
      <th>Red_SEO</th>
      <th>Red_SFS</th>
      <th>Red_SHD</th>
      <th>Red_VAL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Dorado</td>
      <td>Attack</td>
      <td>SFS</td>
      <td>VAL</td>
      <td>0:29</td>
      <td>14</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dorado</td>
      <td>Attack</td>
      <td>SFS</td>
      <td>VAL</td>
      <td>1:05</td>
      <td>24</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>3</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dorado</td>
      <td>Attack</td>
      <td>SFS</td>
      <td>VAL</td>
      <td>1:51</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Dorado</td>
      <td>Attack</td>
      <td>SFS</td>
      <td>VAL</td>
      <td>2:14</td>
      <td>28</td>
      <td>7</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Dorado</td>
      <td>Attack</td>
      <td>SFS</td>
      <td>VAL</td>
      <td>3:24</td>
      <td>17</td>
      <td>4</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 110 columns</p>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Dropping-unusable-columns">Dropping unusable columns</h3><p>I need to make sure that I am only using the columns with numbers in it.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[3]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">df_new</span> <span class="o">=</span> <span class="n">df</span><span class="o">.</span><span class="n">drop</span><span class="p">(</span><span class="n">columns</span><span class="o">=</span><span class="p">[</span><span class="s1">&#39;Map&#39;</span><span class="p">,</span> <span class="s1">&#39;Roundtype&#39;</span><span class="p">,</span> <span class="s1">&#39;FB&#39;</span><span class="p">,</span> <span class="s1">&#39;Winner&#39;</span><span class="p">,</span> <span class="s1">&#39;time&#39;</span><span class="p">,</span> <span class="s1">&#39;Blue_Winner&#39;</span><span class="p">,</span> <span class="s1">&#39;Red_Winner&#39;</span><span class="p">,</span> <span class="s1">&#39;len&#39;</span><span class="p">,</span>
                          <span class="s1">&#39;Blue_Team&#39;</span><span class="p">,</span> <span class="s1">&#39;Red_Team&#39;</span><span class="p">,</span> <span class="s1">&#39;KB&#39;</span><span class="p">,</span> <span class="s1">&#39;KR&#39;</span><span class="p">,</span> <span class="s1">&#39;UB&#39;</span><span class="p">,</span> <span class="s1">&#39;UR&#39;</span><span class="p">,</span> <span class="s1">&#39;FB&#39;</span><span class="p">,</span> <span class="s1">&#39;First_kill&#39;</span><span class="p">,</span> <span class="s1">&#39;Tie&#39;</span><span class="p">])</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Random-Forest-Classification">Random Forest Classification</h2><p>First, I need to prepare my X and y dataframes so that I can easily make a model.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[27]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X</span> <span class="o">=</span> <span class="n">df_new</span>
<span class="n">y</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s1">&#39;Winner&#39;</span><span class="p">]</span><span class="o">.</span><span class="n">map</span><span class="p">({</span><span class="s1">&#39;Blue_Winner&#39;</span><span class="p">:</span><span class="mi">1</span><span class="p">,</span> <span class="s1">&#39;Tie&#39;</span><span class="p">:</span><span class="mi">0</span><span class="p">,</span> <span class="s1">&#39;Red_Winner&#39;</span><span class="p">:</span><span class="o">-</span><span class="mi">1</span><span class="p">})</span>
<span class="n">X_train</span><span class="p">,</span> <span class="n">X_test</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">y_test</span> <span class="o">=</span> <span class="n">train_test_split</span><span class="p">(</span><span class="n">X</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">test_size</span><span class="o">=.</span><span class="mi">2</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Now, it is easy to make a random forest classifier with sklearn.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[28]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1">#Takes a minute to run</span>
<span class="kn">from</span> <span class="nn">sklearn.ensemble</span> <span class="k">import</span> <span class="n">RandomForestClassifier</span>
<span class="n">model</span> <span class="o">=</span> <span class="n">RandomForestClassifier</span><span class="p">(</span><span class="n">n_estimators</span><span class="o">=</span><span class="mi">1000</span><span class="p">)</span>
<span class="n">model</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Let's measure the accuracy of this classifier.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[29]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.metrics</span> <span class="k">import</span> <span class="n">r2_score</span>
<span class="n">y_pred</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">X_test</span><span class="p">)</span>
<span class="n">r2</span> <span class="o">=</span> <span class="n">r2_score</span><span class="p">(</span><span class="n">y_pred</span><span class="p">,</span> <span class="n">y_test</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s1">&#39;R squared: &#39;</span><span class="p">,</span> <span class="n">r2</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>R squared:  -0.789834376743517
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>This can be confusing, so let's look at the results a little more.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[30]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.metrics</span> <span class="k">import</span> <span class="n">confusion_matrix</span>
<span class="kn">import</span> <span class="nn">seaborn</span> <span class="k">as</span> <span class="nn">sns</span>
<span class="n">mat</span> <span class="o">=</span> <span class="n">confusion_matrix</span><span class="p">(</span><span class="n">y_test</span><span class="p">,</span> <span class="n">y_pred</span><span class="p">)</span>
<span class="n">sns</span><span class="o">.</span><span class="n">heatmap</span><span class="p">(</span><span class="n">mat</span><span class="o">.</span><span class="n">T</span><span class="p">,</span> <span class="n">square</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">annot</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">fmt</span><span class="o">=</span><span class="s1">&#39;d&#39;</span><span class="p">,</span> <span class="n">cbar</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s1">&#39;true label&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s1">&#39;predicted label&#39;</span><span class="p">);</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[8]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">sklearn.metrics</span> <span class="k">import</span> <span class="n">accuracy_score</span>
<span class="nb">print</span><span class="p">(</span><span class="s1">&#39;Accuracy: &#39;</span><span class="p">,</span> <span class="n">accuracy_score</span><span class="p">(</span><span class="n">y_pred</span><span class="p">,</span> <span class="n">y_test</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>Accuracy:  0.5272952853598015
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>This isn't much better than just guessing randomly. Let's change the data by removing columns with a tie and see if we get better.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[9]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X</span> <span class="o">=</span> <span class="n">df_new</span><span class="p">[</span><span class="n">y</span> <span class="o">!=</span> <span class="mi">0</span><span class="p">]</span>
<span class="n">y</span> <span class="o">=</span> <span class="n">y</span><span class="p">[</span><span class="n">y</span><span class="o">!=</span> <span class="mi">0</span><span class="p">]</span>
<span class="n">X_train</span><span class="p">,</span> <span class="n">X_test</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">y_test</span> <span class="o">=</span> <span class="n">train_test_split</span><span class="p">(</span><span class="n">X</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">test_size</span><span class="o">=.</span><span class="mi">2</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[10]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1">#Takes a minute to run</span>
<span class="n">model</span> <span class="o">=</span> <span class="n">RandomForestClassifier</span><span class="p">(</span><span class="n">n_estimators</span><span class="o">=</span><span class="mi">1000</span><span class="p">)</span>
<span class="n">model</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[11]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">y_pred</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">X_test</span><span class="p">)</span>
<span class="n">r2</span> <span class="o">=</span> <span class="n">r2_score</span><span class="p">(</span><span class="n">y_pred</span><span class="p">,</span> <span class="n">y_test</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s1">&#39;R squared: &#39;</span><span class="p">,</span> <span class="n">r2</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>R squared:  -0.6604140957082134
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[12]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">mat</span> <span class="o">=</span> <span class="n">confusion_matrix</span><span class="p">(</span><span class="n">y_test</span><span class="p">,</span> <span class="n">y_pred</span><span class="p">)</span>
<span class="n">sns</span><span class="o">.</span><span class="n">heatmap</span><span class="p">(</span><span class="n">mat</span><span class="o">.</span><span class="n">T</span><span class="p">,</span> <span class="n">square</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">annot</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">fmt</span><span class="o">=</span><span class="s1">&#39;d&#39;</span><span class="p">,</span> <span class="n">cbar</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s1">&#39;true label&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s1">&#39;predicted label&#39;</span><span class="p">);</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[13]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="s1">&#39;Accuracy: &#39;</span><span class="p">,</span> <span class="n">accuracy_score</span><span class="p">(</span><span class="n">y_pred</span><span class="p">,</span> <span class="n">y_test</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>Accuracy:  0.5850785340314136
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Neural-Network">Neural Network</h2>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[14]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">keras</span>
<span class="kn">import</span> <span class="nn">tensorflow</span>
<span class="kn">from</span> <span class="nn">keras.models</span> <span class="k">import</span> <span class="n">Sequential</span>
<span class="kn">from</span> <span class="nn">keras.layers</span> <span class="k">import</span> <span class="n">Dense</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stderr output_text">
<pre>/opt/conda/lib/python3.6/site-packages/h5py/__init__.py:36: FutureWarning: Conversion of the second argument of issubdtype from `float` to `np.floating` is deprecated. In future, it will be treated as `np.float64 == np.dtype(float).type`.
  from ._conv import register_converters as _register_converters
Using TensorFlow backend.
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[15]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X_train</span><span class="p">,</span> <span class="n">X_test</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">y_test</span> <span class="o">=</span> <span class="n">train_test_split</span><span class="p">(</span><span class="n">X</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">test_size</span><span class="o">=.</span><span class="mi">2</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[16]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">np_X</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="n">X_train</span><span class="p">)</span>
<span class="n">np_X</span><span class="o">.</span><span class="n">reshape</span><span class="p">((</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">94</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>


<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[17]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">y_nn</span> <span class="o">=</span> <span class="n">y</span><span class="o">.</span><span class="n">map</span><span class="p">({</span><span class="mi">1</span><span class="p">:</span> <span class="mi">1</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">:</span><span class="mi">0</span><span class="p">})</span>
<span class="n">X_nn</span> <span class="o">=</span> <span class="n">X</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[21]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">X_train_nn</span><span class="p">,</span> <span class="n">X_test_nn</span><span class="p">,</span> <span class="n">y_train_nn</span><span class="p">,</span> <span class="n">y_test_nn</span> <span class="o">=</span> <span class="n">train_test_split</span><span class="p">(</span><span class="n">X_nn</span><span class="p">,</span> <span class="n">y_nn</span><span class="p">,</span> <span class="n">test_size</span><span class="o">=.</span><span class="mi">2</span><span class="p">)</span>
<span class="n">model</span> <span class="o">=</span> <span class="n">Sequential</span><span class="p">()</span>
<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">Dense</span><span class="p">(</span><span class="mi">128</span><span class="p">,</span> <span class="n">activation</span> <span class="o">=</span> <span class="s1">&#39;sigmoid&#39;</span><span class="p">,</span> <span class="n">input_dim</span> <span class="o">=</span> <span class="mi">94</span><span class="p">))</span>
<span class="c1">#model.add(Dense(128, activation = &#39;relu&#39;))</span>
<span class="n">model</span><span class="o">.</span><span class="n">add</span><span class="p">(</span><span class="n">Dense</span><span class="p">(</span><span class="mi">1</span><span class="p">,</span> <span class="n">activation</span> <span class="o">=</span> <span class="s1">&#39;sigmoid&#39;</span><span class="p">))</span>

<span class="n">model</span><span class="o">.</span><span class="n">compile</span><span class="p">(</span><span class="n">loss</span> <span class="o">=</span> <span class="s1">&#39;binary_crossentropy&#39;</span><span class="p">,</span>
              <span class="n">optimizer</span> <span class="o">=</span> <span class="s1">&#39;adam&#39;</span><span class="p">,</span>
              <span class="n">metrics</span><span class="o">=</span><span class="p">[</span><span class="s1">&#39;accuracy&#39;</span><span class="p">])</span>
<span class="n">model</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">X_train_nn</span><span class="p">,</span> <span class="n">y_train_nn</span><span class="p">,</span> <span class="n">epochs</span><span class="o">=</span><span class="mi">25</span><span class="p">,</span> <span class="n">batch_size</span> <span class="o">=</span> <span class="mi">32</span><span class="p">)</span>
<span class="n">loss</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">evaluate</span><span class="p">(</span><span class="n">X_test_nn</span><span class="p">,</span> <span class="n">y_test_nn</span><span class="p">,</span> <span class="n">batch_size</span> <span class="o">=</span> <span class="mi">32</span><span class="p">)</span>

<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;</span><span class="se">\n\n</span><span class="s2">Final Loss : </span><span class="si">{}</span><span class="s2">&quot;</span><span class="o">.</span><span class="n">format</span><span class="p">(</span><span class="n">loss</span><span class="p">))</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>Epoch 1/25
3052/3052 [==============================] - 0s 142us/step - loss: 0.6950 - acc: 0.5354
Epoch 2/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6699 - acc: 0.5836
Epoch 3/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6606 - acc: 0.6114
Epoch 4/25
3052/3052 [==============================] - 0s 41us/step - loss: 0.6599 - acc: 0.64
Epoch 5/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6537 - acc: 0.6212
Epoch 6/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6522 - acc: 0.6232
Epoch 7/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6492 - acc: 0.6199
Epoch 8/25
3052/3052 [==============================] - 0s 60us/step - loss: 0.6484 - acc: 0.6291
Epoch 9/25
3052/3052 [==============================] - 0s 46us/step - loss: 0.6489 - acc: 0.6320
Epoch 10/25
3052/3052 [==============================] - 0s 41us/step - loss: 0.6497 - acc: 0.6196
Epoch 11/25
3052/3052 [==============================] - 0s 46us/step - loss: 0.6465 - acc: 0.6261
Epoch 12/25
3052/3052 [==============================] - 0s 47us/step - loss: 0.6517 - acc: 0.6163
Epoch 13/25
3052/3052 [==============================] - 0s 41us/step - loss: 0.6482 - acc: 0.6180
Epoch 14/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6463 - acc: 0.6307
Epoch 15/25
3052/3052 [==============================] - 0s 39us/step - loss: 0.6474 - acc: 0.6288
Epoch 16/25
3052/3052 [==============================] - 0s 41us/step - loss: 0.6462 - acc: 0.6360
Epoch 17/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6455 - acc: 0.6307
Epoch 18/25
3052/3052 [==============================] - 0s 39us/step - loss: 0.6458 - acc: 0.6307
Epoch 19/25
3052/3052 [==============================] - 0s 39us/step - loss: 0.6460 - acc: 0.6314
Epoch 20/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6445 - acc: 0.6337
Epoch 21/25
3052/3052 [==============================] - 0s 64us/step - loss: 0.6444 - acc: 0.6294
Epoch 22/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6456 - acc: 0.6281
Epoch 23/25
3052/3052 [==============================] - 0s 43us/step - loss: 0.6450 - acc: 0.6350
Epoch 24/25
3052/3052 [==============================] - 0s 46us/step - loss: 0.6460 - acc: 0.6275
Epoch 25/25
3052/3052 [==============================] - 0s 40us/step - loss: 0.6466 - acc: 0.6265
764/764 [==============================] - 0s 52us/step


Final Loss : [0.6602976862048604, 0.6020942399014977]
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[22]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">y_pred_nn</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">X_test_nn</span><span class="p">)</span>
<span class="n">y_test_graph</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">round</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">(</span><span class="n">y_test_nn</span><span class="p">))</span>
<span class="n">y_pred_graph</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">round</span><span class="p">(</span><span class="n">y_pred_nn</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[23]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">mat</span> <span class="o">=</span> <span class="n">confusion_matrix</span><span class="p">(</span><span class="n">y_test_graph</span><span class="p">,</span> <span class="n">y_pred_graph</span><span class="p">)</span>
<span class="n">sns</span><span class="o">.</span><span class="n">heatmap</span><span class="p">(</span><span class="n">mat</span><span class="o">.</span><span class="n">T</span><span class="p">,</span> <span class="n">square</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">annot</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">fmt</span><span class="o">=</span><span class="s1">&#39;d&#39;</span><span class="p">,</span> <span class="n">cbar</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">xlabel</span><span class="p">(</span><span class="s1">&#39;true label&#39;</span><span class="p">)</span>
<span class="n">plt</span><span class="o">.</span><span class="n">ylabel</span><span class="p">(</span><span class="s1">&#39;predicted label&#39;</span><span class="p">);</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

<div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>

</div>

</div>
</div>

</div>
 


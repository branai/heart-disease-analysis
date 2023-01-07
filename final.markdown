---
jupyter:
  kernelspec:
    display_name: base
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.9.12
  nbformat: 4
  nbformat_minor: 2
  orig_nbformat: 4
  vscode:
    interpreter:
      hash: 40d3a090f54c6569ab1632332b64b2c03c39dcf918b08424e98f38b5ae0af88f
---

::: {.cell .markdown}
# Classifying Patients With Heart Disease

Author: Bijan Naimi
:::

::: {.cell .markdown}
### Motivation

According to the CDC, approximately 697,000 people per year die of heart
disease in the United States, alone. That puts heart disease as being
the cause of of 1 out of 5 deaths in the US. The fact of the matter is,
however, that a change in lifestyle can very often combat the risk
someone is at for heart disease. Also, if someone already has heart
disease that is undiagnosed, lack of treatment can be unnecesarily
fatal. The problem at hand is just to identify individuals with heart
disease using basic health data.

Heart related illnesses are often referred to as the \"silent killer.\"
It is very often the case where people who suffer from them do not
realize their condition unil it is too late. This is the main motivation
behind choosing this topic as my final project: can we effectively
classify patients with heart disease based off of basic data pertaining
to their health. In addition, it would be a bonus if I can identify a
leading factor that would suggest someone is likely to have heart
disease.
:::

::: {.cell .markdown}
### Data Collection

I found the following dataset to use for this project:
<https://archive.ics.uci.edu/ml/datasets/heart+disease>

I also found that this exact data was uploaded to kaggle.com at the
following link (the original uploader mislabeled the dataset metadata /
title):
<https://www.kaggle.com/datasets/rashikrahmanpritom/heart-attack-analysis-prediction-dataset>

Instead of wrangling the data I from the former link, I used the CSV
from the second one and read it into pandas below.
:::

::: {.cell .markdown}
### Data Management
:::

::: {.cell .markdown}
I first read the CSV into pandas and took a look at the data in a more
organized form that just a raw CSV.
:::

::: {.cell .code execution_count="240"}
``` {.python}
import pandas as pd

# Read into pandas
df = pd.read_csv('./heart.csv')
df
```

::: {.output .execute_result execution_count="240"}
```{=html}
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
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>trtbps</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalachh</th>
      <th>exng</th>
      <th>oldpeak</th>
      <th>slp</th>
      <th>caa</th>
      <th>thall</th>
      <th>output</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>3</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>0</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>37</td>
      <td>1</td>
      <td>2</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>1</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>41</td>
      <td>0</td>
      <td>1</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>0</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>56</td>
      <td>1</td>
      <td>1</td>
      <td>120</td>
      <td>236</td>
      <td>0</td>
      <td>1</td>
      <td>178</td>
      <td>0</td>
      <td>0.8</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>57</td>
      <td>0</td>
      <td>0</td>
      <td>120</td>
      <td>354</td>
      <td>0</td>
      <td>1</td>
      <td>163</td>
      <td>1</td>
      <td>0.6</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>298</th>
      <td>57</td>
      <td>0</td>
      <td>0</td>
      <td>140</td>
      <td>241</td>
      <td>0</td>
      <td>1</td>
      <td>123</td>
      <td>1</td>
      <td>0.2</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>299</th>
      <td>45</td>
      <td>1</td>
      <td>3</td>
      <td>110</td>
      <td>264</td>
      <td>0</td>
      <td>1</td>
      <td>132</td>
      <td>0</td>
      <td>1.2</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>300</th>
      <td>68</td>
      <td>1</td>
      <td>0</td>
      <td>144</td>
      <td>193</td>
      <td>1</td>
      <td>1</td>
      <td>141</td>
      <td>0</td>
      <td>3.4</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>301</th>
      <td>57</td>
      <td>1</td>
      <td>0</td>
      <td>130</td>
      <td>131</td>
      <td>0</td>
      <td>1</td>
      <td>115</td>
      <td>1</td>
      <td>1.2</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>302</th>
      <td>57</td>
      <td>0</td>
      <td>1</td>
      <td>130</td>
      <td>236</td>
      <td>0</td>
      <td>0</td>
      <td>174</td>
      <td>0</td>
      <td>0.0</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>303 rows × 14 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
Here is a breakdown of the significance of each column

age: Age of observation `<br>`{=html} sex: Sex of observation (0 or 1)
`<br>`{=html} cp: Chest pain type (0, 1, 2, 3) `<br>`{=html} trestbps:
Resting blood pressure (mm Hg) `<br>`{=html} chol: Cholestrol (mg/dl)
`<br>`{=html} fbs: Whether or not the observation has a high fasting
blood sugar (0 or 1) `<br>`{=html} restecg: Resting electrocardiographic
results (0, 1, 2) `<br>`{=html} thalach: Maximum heartrate achieved
`<br>`{=html} exng: Exercise induced angina (0 or 1) `<br>`{=html}
oldpeak: Previous peak `<br>`{=html} slope: Slope (0, 1 , 2)
`<br>`{=html} ca: Number of major blood vessels (0, 1 , 2,
3)`<br>`{=html} thal: thall rate (1, 2, 3) `<br>`{=html} output: Heart
disease diagnosis (0, 1)`<br>`{=html}
:::

::: {.cell .markdown}
I noticed how some data was converted from quantitative to qualitative
by the creator of the dataset, for exapmle the fasting blood sugar
attribute is 1 if it is higher than 120 and 0 otherwise. I felt like it
is important to make this distinction. I confirmed there are no missing
values in the data as seen below.
:::

::: {.cell .code execution_count="241"}
``` {.python}
df = df.copy().dropna()
df
```

::: {.output .execute_result execution_count="241"}
```{=html}
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
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>trtbps</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalachh</th>
      <th>exng</th>
      <th>oldpeak</th>
      <th>slp</th>
      <th>caa</th>
      <th>thall</th>
      <th>output</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>3</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>0</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>37</td>
      <td>1</td>
      <td>2</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>1</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>41</td>
      <td>0</td>
      <td>1</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>0</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>56</td>
      <td>1</td>
      <td>1</td>
      <td>120</td>
      <td>236</td>
      <td>0</td>
      <td>1</td>
      <td>178</td>
      <td>0</td>
      <td>0.8</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>57</td>
      <td>0</td>
      <td>0</td>
      <td>120</td>
      <td>354</td>
      <td>0</td>
      <td>1</td>
      <td>163</td>
      <td>1</td>
      <td>0.6</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>298</th>
      <td>57</td>
      <td>0</td>
      <td>0</td>
      <td>140</td>
      <td>241</td>
      <td>0</td>
      <td>1</td>
      <td>123</td>
      <td>1</td>
      <td>0.2</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>299</th>
      <td>45</td>
      <td>1</td>
      <td>3</td>
      <td>110</td>
      <td>264</td>
      <td>0</td>
      <td>1</td>
      <td>132</td>
      <td>0</td>
      <td>1.2</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>300</th>
      <td>68</td>
      <td>1</td>
      <td>0</td>
      <td>144</td>
      <td>193</td>
      <td>1</td>
      <td>1</td>
      <td>141</td>
      <td>0</td>
      <td>3.4</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>301</th>
      <td>57</td>
      <td>1</td>
      <td>0</td>
      <td>130</td>
      <td>131</td>
      <td>0</td>
      <td>1</td>
      <td>115</td>
      <td>1</td>
      <td>1.2</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>302</th>
      <td>57</td>
      <td>0</td>
      <td>1</td>
      <td>130</td>
      <td>236</td>
      <td>0</td>
      <td>0</td>
      <td>174</td>
      <td>0</td>
      <td>0.0</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>303 rows × 14 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
### Exploratory Data Analysis
:::

::: {.cell .markdown}
To get a better idea of how everything is distributed, I made a
histogram of each attribute using matplotlib.
:::

::: {.cell .code execution_count="242"}
``` {.python}
import numpy as np
import matplotlib.pyplot as plt

# Setting a universal figure size
plt.rcParams['figure.figsize'] = [5.0, 5.0]
plt.rcParams['figure.dpi'] = 140

# For each column make one plot
for col in df.columns:
    # Create histogram with column data
    plt.hist(df[col])
    plt.title('Histogram for Column "' + col + '"')
    plt.xlabel("Value")
    plt.ylabel("Frequency")
    plt.show()
```

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/9daac0470cf79db29964ad36bea17a57fc3f231c.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/40e81087a6181143e5dcf59f7ddcf994bb79bd9d.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/bfa3d60eacded3d50c01f702085c9685ce3538e7.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/ac8c38f5da2b016c34a995df55e5008a858da92f.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/5c18f59e313fc0740e30ef37450a9805e3e26b8c.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/dcb4c5985997117fd5feaffc03dd98cf4e559e07.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/f7292cac4a635a995d4f2293f6dc85f36cf8aeed.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/454c52e7c4767060a846a69c876a093b67accec2.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/1e2199f06a34ffd774f9d7f7cdb6ffa04d0349b4.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/797721c7a554be3027823b0119de481da1360093.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/7b4e0f29c3dd81ea1119f59a2ad0297568f466a7.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/6a17d14c85a0f13ff9d138fce5406e2517370e24.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/ad7b3b6f0fadb05b364d2f64a921598b31d6cb0b.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/a9f338833e4f36e8a061c1af73b55e6f120cdd4a.png)
:::
:::

::: {.cell .markdown}
I did not see anything in the distributions out of the norm. I was only
really able to see the frequency of some of the categorical data that I
was not familiar with before, which will be useful in the future. To get
an idea of how the attributes interact with one another, I decided to
make a heatmap as the next step in my EDA. I used seaborn, a data
visualization library, to do so.
:::

::: {.cell .code execution_count="243"}
``` {.python}
import seaborn as sns

# Get a correlation matrix from the dataframe and use it in seaborn
corr_matrix = df.corr()
ax = plt.axes()

# Use correlation matrix in heatmap
sns.heatmap(corr_matrix, annot=False , linewidths=.5, ax=ax)
ax.set_title('Correlation Heatmap for Data Attributes')
```

::: {.output .execute_result execution_count="243"}
    Text(0.5, 1.0, 'Correlation Heatmap for Data Attributes')
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/3bc1782c7888e57c91c89fad1f201550b7fb09e5.png)
:::
:::

::: {.cell .markdown}
I was pretty suprised to see how some of the variables correlated to the
output variable and how some did not.

I expected correlation from the maximum heartrate achieved, which there
was, but I thought it was really interesting how the type of chest pain
was one of the highest correlated variables to heart disease.

However, I intuitively thought that things like age and blood pressure
would play more of a factor in heart disease, but these were only weakly
correlated.

Another big correlator to output was maximum heartrate achieved
(thalachh). To dig deeper, I took a look at how maximum heartrate
achieved linearly correlates to the other quantitative variables.
:::

::: {.cell .code execution_count="244"}
``` {.python}
from sklearn.linear_model import LinearRegression

# maximum heartrate achieved to be x axis
x_bloodpressure = df['thalachh']

# List of each variable for y
y_list = ['age', 'chol', 'trtbps', 'oldpeak']

for curr in y_list:
    # Get column for that variable
    y = df[curr]

    # Reshape maximum heartrate achieved
    x = np.array(x_bloodpressure).reshape((-1, 1))
    y = np.array(y)

    # Make linear resgression model
    reg = LinearRegression().fit(x, y)
    y = reg.intercept_ + (reg.coef_[0] * x)

    # Plot x and y and also draw linear regression line
    plt.plot(x, y, '-r')
    plt.plot(x_bloodpressure, df[curr], '.')
    plt.xlabel("Maximum Heartrate Achieved")
    plt.ylabel(curr)
    plt.title("Data in Column " + curr + " Against maximum heartrate achieved")
    plt.show()
```

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/b10f0ccd0e40a7b1556cfb283947f9bf5c60e311.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/fcc91657e866f899a92a69ee894ed8aabc4d71fd.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/4f33f4bc7836e9793fc45f13644e8646d62822f7.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/0c4bd19b71752368608d1c121b38bc4dd8355325.png)
:::
:::

::: {.cell .markdown}
Up until this point, I have not directly distinguish between positive
and negative diagnoses when looking into the variables, so I decided to
use violin plots to do so, in order to see if there is anything
anomolous in their distributions when seperated in this way.
:::

::: {.cell .code execution_count="245"}
``` {.python}
# To seperate based off of classification
data_for_target = {
    0: [],
    1: []
}

# EXCLUDE THE OUTPUT VARIABLE
cols = list(df.columns)[:-1]

for c in cols:
    for index, row in df.iterrows():
        data_for_target[row['output']].append(row[c])

    plt.violinplot([data_for_target[0], data_for_target[1]], [0,1])
    plt.title("Distributions of " + c + " for Each Heart Disease Diagnosis")
    plt.ylabel("Values")
    plt.xlabel("Heart Disease Diagnosis (0 = Negative, 1 = Positive)")
    plt.show()
```

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/de6d9ade9f2e16d842952fda7e51b6142c610cea.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/3692c05dbaf4de6711b18ca42033844c8721aa34.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/1618e21b1c463e9fb9477316b2fc729761d05ace.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/10d956b0005228ee7620f36122c3341f39720eb8.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/9eafd363eed977869fde973ef06ab64def02f662.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/9b17b57ac7c76eb1c55eed5a65cfcbeed84c9443.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/6282d91ffd92c3681088d4adb0004398b2c4517b.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/39c651c2c79f1f9e2c5bebec5fca6d519f3de056.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/cc6298e9d4812e57b0a78571defa56e9f6279b3f.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/f9870aacea9a488e478333bfaeaaa27e8ce452ce.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/493cd6d5b1f8434cb403cae342b60d4e03250367.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/4ced04c04855dc9144e4dee07e27297c28980587.png)
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/64334412bac45bb0ecbc304d023a4699d66f407f.png)
:::
:::

::: {.cell .markdown}
From these graphs we can see that the distributions of these variables
are almost identical when seperating between positive and negative
diagnoses.
:::

::: {.cell .markdown}
### Hypothesis Testing
:::

::: {.cell .markdown}
When performing the linear regression on the quantitative variables in
the data, I noticed a correlation between age and maximum heartrate
achieved. My thinking is that if I could find some correlations between
quantitative values in the data with a variable that is extremely
correlative to heart disease diagnoses, this information could be used
to help predict the classification of heart disease. To further
investigate this, I performed a hypothesis test on the two variable\'s
linear correlation using the statsmodels library. I will be using the
least squares method of evaluation.

$$H_{0} : \beta_1 = 0$$ $$H_{a} : \beta_1 \ne 0$$
:::

::: {.cell .markdown}
The null hypothesis is essentially that the linear model does not work
and the alternate hypothesis is that the slope (B1) is non-zero.
:::

::: {.cell .code execution_count="246"}
``` {.python}
import statsmodels.api as sm

# List of variables to run hypothesis test with heartrate
y_list = ['age', 'chol', 'trtbps', 'oldpeak']

for curr in y_list:
    X = list(df['thalachh'])
    X = sm.add_constant(X)
    y = list(df[curr])

    # Run least squares hypothesis test
    model = sm.OLS(y, X)
    results = model.fit()
    
    # Print out data we want from test
    print(results.summary())
```

::: {.output .stream .stdout}
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.159
    Model:                            OLS   Adj. R-squared:                  0.156
    Method:                 Least Squares   F-statistic:                     56.83
    Date:                Fri, 16 Dec 2022   Prob (F-statistic):           5.63e-13
    Time:                        23:23:23   Log-Likelihood:                -1071.7
    No. Observations:                 303   AIC:                             2147.
    Df Residuals:                     301   BIC:                             2155.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const         78.0132      3.173     24.585      0.000      71.769      84.258
    x1            -0.1580      0.021     -7.539      0.000      -0.199      -0.117
    ==============================================================================
    Omnibus:                        6.417   Durbin-Watson:                   2.157
    Prob(Omnibus):                  0.040   Jarque-Bera (JB):                4.018
    Skew:                          -0.091   Prob(JB):                        0.134
    Kurtosis:                       2.466   Cond. No.                     1.00e+03
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large,  1e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.000
    Model:                            OLS   Adj. R-squared:                 -0.003
    Method:                 Least Squares   F-statistic:                   0.02974
    Date:                Fri, 16 Dec 2022   Prob (F-statistic):              0.863
    Time:                        23:23:23   Log-Likelihood:                -1625.7
    No. Observations:                 303   AIC:                             3255.
    Df Residuals:                     301   BIC:                             3263.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const        249.6299     19.744     12.644      0.000     210.777     288.483
    x1            -0.0225      0.130     -0.172      0.863      -0.279       0.234
    ==============================================================================
    Omnibus:                       83.725   Durbin-Watson:                   2.032
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              313.571
    Skew:                           1.139   Prob(JB):                     8.11e-69
    Kurtosis:                       7.433   Cond. No.                     1.00e+03
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large,  1e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.002
    Model:                            OLS   Adj. R-squared:                 -0.001
    Method:                 Least Squares   F-statistic:                    0.6578
    Date:                Fri, 16 Dec 2022   Prob (F-statistic):              0.418
    Time:                        23:23:23   Log-Likelihood:                -1297.0
    No. Observations:                 303   AIC:                             2598.
    Df Residuals:                     301   BIC:                             2605.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const        136.9745      6.674     20.524      0.000     123.841     150.108
    x1            -0.0358      0.044     -0.811      0.418      -0.123       0.051
    ==============================================================================
    Omnibus:                       28.268   Durbin-Watson:                   1.795
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):               35.244
    Skew:                           0.703   Prob(JB):                     2.22e-08
    Kurtosis:                       3.904   Cond. No.                     1.00e+03
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large,  1e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.118
    Model:                            OLS   Adj. R-squared:                  0.116
    Method:                 Least Squares   F-statistic:                     40.45
    Date:                Fri, 16 Dec 2022   Prob (F-statistic):           7.48e-10
    Time:                        23:23:23   Log-Likelihood:                -455.59
    No. Observations:                 303   AIC:                             915.2
    Df Residuals:                     301   BIC:                             922.6
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const          3.6505      0.415      8.790      0.000       2.833       4.468
    x1            -0.0174      0.003     -6.360      0.000      -0.023      -0.012
    ==============================================================================
    Omnibus:                       74.222   Durbin-Watson:                   1.672
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              138.482
    Skew:                           1.309   Prob(JB):                     8.49e-31
    Kurtosis:                       5.027   Cond. No.                     1.00e+03
    ==============================================================================

    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large,  1e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
:::
:::

::: {.cell .markdown}
We can reject the null hypothesis for age and oldpeak with a
significance of 0.05, since the P values are less than 0.05 for these
variables. This means there is a proven correlation between these
variables and maximum heartrate achieved.
:::

::: {.cell .markdown}
### Classifications
:::

::: {.cell .markdown}
Finally, I am going to see if I can produce a model to accurately
classify an observation\'s heart disease diagnosis based on the features
I have analyzed thus far. I will be using 33 percent of the data at hand
as testing data and the rest to train the models.

As for the models, I will be using Logistic Regression, Decision Trees,
and Random Forest. To evaluate them, we will use both accuracy and false
negatives, since classifying someone as not having heart disease when
they do is pretty severe.
:::

::: {.cell .markdown}
In the following code, I seperate the predicting column from everything
else and split the training and testing data using built in functions to
sklearn.
:::

::: {.cell .code execution_count="247"}
``` {.python}
X = []
y = []

cols = list(df.columns)[:-1] # Cols is all of the non-target data

for index, row in df.iterrows():
    to_append = []
    for c in cols:
        to_append.append(row[c])
    X.append(to_append) # All of the data in cols
    y.append(row['output']) # Our target variable

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42) # Training data and testing data split
```
:::

::: {.cell .markdown}
Now that I have training and testing data, I will train a logistic
regression model using sklearn. Logistic regression is perfect for this
task, since it takes in both categorical and continuous quantiative
values to produce a binary result, which is exactly what we want.
:::

::: {.cell .code execution_count="248"}
``` {.python}
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler

clf = LogisticRegression() # Using logistic regression

scaler = StandardScaler()
X_train = scaler.fit_transform(X_train) # Transform training data using scalar
X_test = scaler.transform(X_test)

clf.fit(X_train,y_train) # Fit the model using trainnig data

result = clf.predict(X_test) # Get results with test data

acc = 0
false_neg = 0
for i in range(len(result)):
    if (y_test[i] == 1 and result[i] == 0) == False: # A false negative
        false_neg += 1
    if y_test[i] == result[i]:
        acc += 1

print("Accuracy", acc/len(result))
print("(Not a) False Negative Rate", false_neg/len(result))
```

::: {.output .stream .stdout}
    Accuracy 0.81
    (Not a) False Negative Rate 0.9
:::
:::

::: {.cell .markdown}
I also tried using a decision tree. Decision trees are an example of a
classifying algorithm that is more organic. We are essentially
seperating data points into different groups repeatedly using the
different features we are supplying the model.
:::

::: {.cell .code execution_count="249"}
``` {.python}
from sklearn import tree
clf = tree.DecisionTreeClassifier() # Using decision trees
clf.fit(X_train,y_train)

result = clf.predict(X_test)

acc = 0
false_neg = 0
for i in range(len(result)):
    if (y_test[i] == 1 and result[i] == 0) == False:
        false_neg += 1
    if y_test[i] == result[i]:
        acc += 1

print("Accuracy", acc/len(result))
print("(Not a) False Negative Rate", false_neg/len(result))

tree.plot_tree(clf)
```

::: {.output .stream .stdout}
    Accuracy 0.73
    (Not a) False Negative Rate 0.82
:::

::: {.output .execute_result execution_count="249"}
    [Text(0.5492021276595744, 0.95, 'X[11] <= -0.148\ngini = 0.499\nsamples = 203\nvalue = [96, 107]'),
     Text(0.3696808510638298, 0.85, 'X[12] <= 0.284\ngini = 0.387\nsamples = 122\nvalue = [32, 90]'),
     Text(0.2393617021276596, 0.75, 'X[9] <= 0.607\ngini = 0.191\nsamples = 84\nvalue = [9, 75]'),
     Text(0.1595744680851064, 0.65, 'X[3] <= 2.125\ngini = 0.101\nsamples = 75\nvalue = [4, 71]'),
     Text(0.10638297872340426, 0.55, 'X[0] <= 0.65\ngini = 0.079\nsamples = 73\nvalue = [3, 70]'),
     Text(0.06382978723404255, 0.45, 'X[3] <= -1.281\ngini = 0.032\nsamples = 61\nvalue = [1, 60]'),
     Text(0.0425531914893617, 0.35, 'X[1] <= -0.338\ngini = 0.375\nsamples = 4\nvalue = [1, 3]'),
     Text(0.02127659574468085, 0.25, 'gini = 0.0\nsamples = 3\nvalue = [0, 3]'),
     Text(0.06382978723404255, 0.25, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.0851063829787234, 0.35, 'gini = 0.0\nsamples = 57\nvalue = [0, 57]'),
     Text(0.14893617021276595, 0.45, 'X[9] <= -0.758\ngini = 0.278\nsamples = 12\nvalue = [2, 10]'),
     Text(0.1276595744680851, 0.35, 'X[3] <= 0.099\ngini = 0.48\nsamples = 5\nvalue = [2, 3]'),
     Text(0.10638297872340426, 0.25, 'X[7] <= -0.007\ngini = 0.444\nsamples = 3\nvalue = [2, 1]'),
     Text(0.0851063829787234, 0.15, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.1276595744680851, 0.15, 'X[0] <= 0.761\ngini = 0.5\nsamples = 2\nvalue = [1, 1]'),
     Text(0.10638297872340426, 0.05, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.14893617021276595, 0.05, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.14893617021276595, 0.25, 'gini = 0.0\nsamples = 2\nvalue = [0, 2]'),
     Text(0.1702127659574468, 0.35, 'gini = 0.0\nsamples = 7\nvalue = [0, 7]'),
     Text(0.2127659574468085, 0.55, 'X[0] <= 0.761\ngini = 0.5\nsamples = 2\nvalue = [1, 1]'),
     Text(0.19148936170212766, 0.45, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.23404255319148937, 0.45, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.3191489361702128, 0.65, 'X[0] <= 0.761\ngini = 0.494\nsamples = 9\nvalue = [5, 4]'),
     Text(0.2978723404255319, 0.55, 'X[9] <= 2.199\ngini = 0.278\nsamples = 6\nvalue = [5, 1]'),
     Text(0.2765957446808511, 0.45, 'gini = 0.0\nsamples = 5\nvalue = [5, 0]'),
     Text(0.3191489361702128, 0.45, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.3404255319148936, 0.55, 'gini = 0.0\nsamples = 3\nvalue = [0, 3]'),
     Text(0.5, 0.75, 'X[8] <= 0.361\ngini = 0.478\nsamples = 38\nvalue = [23, 15]'),
     Text(0.425531914893617, 0.65, 'X[0] <= -0.517\ngini = 0.472\nsamples = 21\nvalue = [8, 13]'),
     Text(0.3829787234042553, 0.55, 'X[5] <= 1.113\ngini = 0.375\nsamples = 8\nvalue = [6, 2]'),
     Text(0.3617021276595745, 0.45, 'gini = 0.0\nsamples = 6\nvalue = [6, 0]'),
     Text(0.40425531914893614, 0.45, 'gini = 0.0\nsamples = 2\nvalue = [0, 2]'),
     Text(0.46808510638297873, 0.55, 'X[7] <= 0.148\ngini = 0.26\nsamples = 13\nvalue = [2, 11]'),
     Text(0.44680851063829785, 0.45, 'X[7] <= -0.405\ngini = 0.48\nsamples = 5\nvalue = [2, 3]'),
     Text(0.425531914893617, 0.35, 'gini = 0.0\nsamples = 2\nvalue = [0, 2]'),
     Text(0.46808510638297873, 0.35, 'X[0] <= 0.094\ngini = 0.444\nsamples = 3\nvalue = [2, 1]'),
     Text(0.44680851063829785, 0.25, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.48936170212765956, 0.25, 'gini = 0.0\nsamples = 2\nvalue = [2, 0]'),
     Text(0.48936170212765956, 0.45, 'gini = 0.0\nsamples = 8\nvalue = [0, 8]'),
     Text(0.574468085106383, 0.65, 'X[9] <= -0.849\ngini = 0.208\nsamples = 17\nvalue = [15, 2]'),
     Text(0.5531914893617021, 0.55, 'X[4] <= 0.101\ngini = 0.444\nsamples = 3\nvalue = [1, 2]'),
     Text(0.5319148936170213, 0.45, 'gini = 0.0\nsamples = 2\nvalue = [0, 2]'),
     Text(0.574468085106383, 0.45, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.5957446808510638, 0.55, 'gini = 0.0\nsamples = 14\nvalue = [14, 0]'),
     Text(0.7287234042553191, 0.85, 'X[2] <= -0.49\ngini = 0.332\nsamples = 81\nvalue = [64, 17]'),
     Text(0.6382978723404256, 0.75, 'X[4] <= 0.958\ngini = 0.041\nsamples = 48\nvalue = [47, 1]'),
     Text(0.6170212765957447, 0.65, 'gini = 0.0\nsamples = 40\nvalue = [40, 0]'),
     Text(0.6595744680851063, 0.65, 'X[4] <= 1.003\ngini = 0.219\nsamples = 8\nvalue = [7, 1]'),
     Text(0.6382978723404256, 0.55, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.6808510638297872, 0.55, 'gini = 0.0\nsamples = 7\nvalue = [7, 0]'),
     Text(0.8191489361702128, 0.75, 'X[10] <= 0.156\ngini = 0.5\nsamples = 33\nvalue = [17, 16]'),
     Text(0.7446808510638298, 0.65, 'X[0] <= 0.761\ngini = 0.32\nsamples = 15\nvalue = [12, 3]'),
     Text(0.723404255319149, 0.55, 'gini = 0.0\nsamples = 7\nvalue = [7, 0]'),
     Text(0.7659574468085106, 0.55, 'X[7] <= -1.068\ngini = 0.469\nsamples = 8\nvalue = [5, 3]'),
     Text(0.7446808510638298, 0.45, 'gini = 0.0\nsamples = 4\nvalue = [4, 0]'),
     Text(0.7872340425531915, 0.45, 'X[7] <= 0.59\ngini = 0.375\nsamples = 4\nvalue = [1, 3]'),
     Text(0.7659574468085106, 0.35, 'gini = 0.0\nsamples = 3\nvalue = [0, 3]'),
     Text(0.8085106382978723, 0.35, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.8936170212765957, 0.65, 'X[11] <= 0.942\ngini = 0.401\nsamples = 18\nvalue = [5, 13]'),
     Text(0.851063829787234, 0.55, 'X[3] <= 2.653\ngini = 0.165\nsamples = 11\nvalue = [1, 10]'),
     Text(0.8297872340425532, 0.45, 'gini = 0.0\nsamples = 10\nvalue = [0, 10]'),
     Text(0.8723404255319149, 0.45, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.9361702127659575, 0.55, 'X[7] <= 0.789\ngini = 0.49\nsamples = 7\nvalue = [4, 3]'),
     Text(0.9148936170212766, 0.45, 'gini = 0.0\nsamples = 3\nvalue = [3, 0]'),
     Text(0.9574468085106383, 0.45, 'X[9] <= 0.88\ngini = 0.375\nsamples = 4\nvalue = [1, 3]'),
     Text(0.9361702127659575, 0.35, 'gini = 0.0\nsamples = 3\nvalue = [0, 3]'),
     Text(0.9787234042553191, 0.35, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]')]
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/41109128805832ff7f1aa72a953f4752a03a0be6.png)
:::
:::

::: {.cell .markdown}
The issue with decision trees is that we would be forcing analysis using
variables that we saw, from our Exploratory Data Analysis, are not
necesarily correlated to the diagnosis. Random Forest Classifiers solve
this problem by creating a bunch of decision trees with the features we
supply and finds the best one after training them all.
:::

::: {.cell .code execution_count="250"}
``` {.python}
from sklearn.ensemble import RandomForestClassifier
clf = RandomForestClassifier() # Using random forest
clf.fit(X_train,y_train)

result = clf.predict(X_test)

acc = 0
false_neg = 0
for i in range(len(result)):
    if (y_test[i] == 1 and result[i] == 0) == False:
        false_neg += 1
    if y_test[i] == result[i]:
        acc += 1

print("Accuracy", acc/len(result))
print("(Not a) False Negative Rate", false_neg/len(result))

tree.plot_tree(clf.estimators_[0], feature_names=df.columns, filled=True)
```

::: {.output .stream .stdout}
    Accuracy 0.81
    (Not a) False Negative Rate 0.89
:::

::: {.output .execute_result execution_count="250"}
    [Text(0.6869904891304348, 0.9583333333333334, 'thall <= 0.284\ngini = 0.487\nsamples = 128\nvalue = [85, 118]'),
     Text(0.5478940217391305, 0.875, 'oldpeak <= 0.971\ngini = 0.328\nsamples = 77\nvalue = [25, 96]'),
     Text(0.46535326086956524, 0.7916666666666666, 'chol <= -0.947\ngini = 0.28\nsamples = 70\nvalue = [19, 94]'),
     Text(0.4436141304347826, 0.7083333333333334, 'gini = 0.0\nsamples = 10\nvalue = [0, 22]'),
     Text(0.48709239130434784, 0.7083333333333334, 'thalachh <= 1.032\ngini = 0.33\nsamples = 60\nvalue = [19, 72]'),
     Text(0.38722826086956524, 0.625, 'caa <= 0.942\ngini = 0.287\nsamples = 49\nvalue = [13, 62]'),
     Text(0.296195652173913, 0.5416666666666666, 'oldpeak <= -0.803\ngini = 0.242\nsamples = 45\nvalue = [10, 61]'),
     Text(0.22282608695652173, 0.4583333333333333, 'trtbps <= -0.87\ngini = 0.355\nsamples = 24\nvalue = [9, 30]'),
     Text(0.14130434782608695, 0.375, 'chol <= 0.119\ngini = 0.497\nsamples = 8\nvalue = [6, 7]'),
     Text(0.08695652173913043, 0.2916666666666667, 'thalachh <= 0.126\ngini = 0.469\nsamples = 5\nvalue = [5, 3]'),
     Text(0.043478260869565216, 0.20833333333333334, 'thalachh <= -0.537\ngini = 0.32\nsamples = 3\nvalue = [4, 1]'),
     Text(0.021739130434782608, 0.125, 'gini = 0.0\nsamples = 1\nvalue = [2, 0]'),
     Text(0.06521739130434782, 0.125, 'thalachh <= -0.095\ngini = 0.444\nsamples = 2\nvalue = [2, 1]'),
     Text(0.043478260869565216, 0.041666666666666664, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.08695652173913043, 0.041666666666666664, 'gini = 0.0\nsamples = 1\nvalue = [2, 0]'),
     Text(0.13043478260869565, 0.20833333333333334, 'caa <= -0.148\ngini = 0.444\nsamples = 2\nvalue = [1, 2]'),
     Text(0.10869565217391304, 0.125, 'gini = 0.0\nsamples = 1\nvalue = [0, 2]'),
     Text(0.15217391304347827, 0.125, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.1956521739130435, 0.2916666666666667, 'thalachh <= 0.457\ngini = 0.32\nsamples = 3\nvalue = [1, 4]'),
     Text(0.17391304347826086, 0.20833333333333334, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.21739130434782608, 0.20833333333333334, 'gini = 0.0\nsamples = 2\nvalue = [0, 4]'),
     Text(0.30434782608695654, 0.375, 'caa <= -0.148\ngini = 0.204\nsamples = 16\nvalue = [3, 23]'),
     Text(0.2826086956521739, 0.2916666666666667, 'age <= 0.428\ngini = 0.236\nsamples = 13\nvalue = [3, 19]'),
     Text(0.2608695652173913, 0.20833333333333334, 'gini = 0.0\nsamples = 9\nvalue = [0, 16]'),
     Text(0.30434782608695654, 0.20833333333333334, 'trtbps <= -0.224\ngini = 0.5\nsamples = 4\nvalue = [3, 3]'),
     Text(0.2826086956521739, 0.125, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.32608695652173914, 0.125, 'cp <= -0.49\ngini = 0.48\nsamples = 3\nvalue = [3, 2]'),
     Text(0.30434782608695654, 0.041666666666666664, 'gini = 0.0\nsamples = 2\nvalue = [3, 0]'),
     Text(0.34782608695652173, 0.041666666666666664, 'gini = 0.0\nsamples = 1\nvalue = [0, 2]'),
     Text(0.32608695652173914, 0.2916666666666667, 'gini = 0.0\nsamples = 3\nvalue = [0, 4]'),
     Text(0.3695652173913043, 0.4583333333333333, 'thalachh <= -1.333\ngini = 0.061\nsamples = 21\nvalue = [1, 31]'),
     Text(0.34782608695652173, 0.375, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.391304347826087, 0.375, 'gini = 0.0\nsamples = 20\nvalue = [0, 31]'),
     Text(0.4782608695652174, 0.5416666666666666, 'age <= 1.095\ngini = 0.375\nsamples = 4\nvalue = [3, 1]'),
     Text(0.45652173913043476, 0.4583333333333333, 'chol <= 0.684\ngini = 0.5\nsamples = 2\nvalue = [1, 1]'),
     Text(0.43478260869565216, 0.375, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.4782608695652174, 0.375, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.5, 0.4583333333333333, 'gini = 0.0\nsamples = 2\nvalue = [2, 0]'),
     Text(0.5869565217391305, 0.625, 'oldpeak <= 0.243\ngini = 0.469\nsamples = 11\nvalue = [6, 10]'),
     Text(0.5652173913043478, 0.5416666666666666, 'chol <= -0.737\ngini = 0.408\nsamples = 10\nvalue = [4, 10]'),
     Text(0.5434782608695652, 0.4583333333333333, 'gini = 0.0\nsamples = 1\nvalue = [3, 0]'),
     Text(0.5869565217391305, 0.4583333333333333, 'caa <= -0.148\ngini = 0.165\nsamples = 9\nvalue = [1, 10]'),
     Text(0.5652173913043478, 0.375, 'gini = 0.0\nsamples = 7\nvalue = [0, 9]'),
     Text(0.6086956521739131, 0.375, 'trtbps <= 0.481\ngini = 0.5\nsamples = 2\nvalue = [1, 1]'),
     Text(0.5869565217391305, 0.2916666666666667, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.6304347826086957, 0.2916666666666667, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.6086956521739131, 0.5416666666666666, 'gini = 0.0\nsamples = 1\nvalue = [2, 0]'),
     Text(0.6304347826086957, 0.7916666666666666, 'thall <= -1.385\ngini = 0.375\nsamples = 7\nvalue = [6, 2]'),
     Text(0.6086956521739131, 0.7083333333333334, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.6521739130434783, 0.7083333333333334, 'age <= -0.961\ngini = 0.245\nsamples = 6\nvalue = [6, 1]'),
     Text(0.6304347826086957, 0.625, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.6739130434782609, 0.625, 'gini = 0.0\nsamples = 5\nvalue = [6, 0]'),
     Text(0.8260869565217391, 0.875, 'trtbps <= -1.281\ngini = 0.393\nsamples = 51\nvalue = [60, 22]'),
     Text(0.8043478260869565, 0.7916666666666666, 'gini = 0.0\nsamples = 1\nvalue = [0, 2]'),
     Text(0.8478260869565217, 0.7916666666666666, 'thalachh <= 0.413\ngini = 0.375\nsamples = 50\nvalue = [60, 20]'),
     Text(0.7608695652173914, 0.7083333333333334, 'age <= 0.928\ngini = 0.142\nsamples = 34\nvalue = [48, 4]'),
     Text(0.717391304347826, 0.625, 'trtbps <= 0.598\ngini = 0.045\nsamples = 28\nvalue = [42, 1]'),
     Text(0.6956521739130435, 0.5416666666666666, 'gini = 0.0\nsamples = 24\nvalue = [34, 0]'),
     Text(0.7391304347826086, 0.5416666666666666, 'chol <= -0.118\ngini = 0.198\nsamples = 4\nvalue = [8, 1]'),
     Text(0.717391304347826, 0.4583333333333333, 'slp <= 0.156\ngini = 0.5\nsamples = 2\nvalue = [1, 1]'),
     Text(0.6956521739130435, 0.375, 'gini = 0.0\nsamples = 1\nvalue = [1, 0]'),
     Text(0.7391304347826086, 0.375, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.7608695652173914, 0.4583333333333333, 'gini = 0.0\nsamples = 2\nvalue = [7, 0]'),
     Text(0.8043478260869565, 0.625, 'chol <= 0.493\ngini = 0.444\nsamples = 6\nvalue = [6, 3]'),
     Text(0.782608695652174, 0.5416666666666666, 'gini = 0.0\nsamples = 4\nvalue = [6, 0]'),
     Text(0.8260869565217391, 0.5416666666666666, 'gini = 0.0\nsamples = 2\nvalue = [0, 3]'),
     Text(0.9347826086956522, 0.7083333333333334, 'chol <= 0.857\ngini = 0.49\nsamples = 16\nvalue = [12, 16]'),
     Text(0.9130434782608695, 0.625, 'age <= -0.183\ngini = 0.496\nsamples = 14\nvalue = [12, 10]'),
     Text(0.8695652173913043, 0.5416666666666666, 'fbs <= 1.113\ngini = 0.219\nsamples = 5\nvalue = [7, 1]'),
     Text(0.8478260869565217, 0.4583333333333333, 'gini = 0.0\nsamples = 4\nvalue = [7, 0]'),
     Text(0.8913043478260869, 0.4583333333333333, 'gini = 0.0\nsamples = 1\nvalue = [0, 1]'),
     Text(0.9565217391304348, 0.5416666666666666, 'caa <= -0.148\ngini = 0.459\nsamples = 9\nvalue = [5, 9]'),
     Text(0.9347826086956522, 0.4583333333333333, 'gini = 0.0\nsamples = 5\nvalue = [0, 9]'),
     Text(0.9782608695652174, 0.4583333333333333, 'gini = 0.0\nsamples = 4\nvalue = [5, 0]'),
     Text(0.9565217391304348, 0.625, 'gini = 0.0\nsamples = 2\nvalue = [0, 6]')]
:::

::: {.output .display_data}
![](images_e37a13d371c94173a970d2305f3a1643/4807be66bb266eef8fe0e28333390239daa189a5.png)
:::
:::

::: {.cell .markdown}
From the resulting evaluations, we can see that the Decision tree
performs significantly more poorly than Random Forest and Logistic
Regression. Random Forest and Logistic Regression are pretty close in
their accuracy, with Random forest\'s false negative rate being slightly
better.
:::

::: {.cell .markdown}
### Insights Gained
:::

::: {.cell .markdown}
I was able to create a model to predict heart disease in our
observations with 80-90% effectiveness. I also recognized that when
comparing simliarly effective models, a higher false negative rate
should be taken more seriously than other forms of evaluation, since
falsely giving someone a negative diagnosis is worse than the opposite.

I found some correlations between other variables provided in the
dataset outside of the diagnosis variable. Concerning the diagnosis
variable however, as suggested from the exploratory data analysis and
the build of the random forest model, the leading factor that suggests
heart disease is maximum heartrate achieved, which is the maximum
heartrate produced by the body when under maximum stress.
:::

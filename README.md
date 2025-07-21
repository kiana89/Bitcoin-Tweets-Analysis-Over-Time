## Bitcoin Tweets Analysis with Kaggle

This project focuses on extracting and preparing a dataset of Bitcoin-related tweets for further analysis using **GPU-accelerated computing with RAPIDS cuDF** and visualization tools like **Matplotlib** and **Seaborn**.

### Installation & Setup

1. **Clone RAPIDS Utilities**
   We clone the RAPIDS CSP utility repository and run a script to install required RAPIDS packages on Colab.

   ```python
   !git clone https://github.com/rapidsai/rapidsai-csp-utils.git
   !python rapidsai-csp-utils/colab/pip-install.py
   ```

2. **Install Kaggle API**
   Quietly install the Kaggle Python package to download datasets.

   ```python
   !pip install kaggle --quiet
   ```

3. **Set Up Kaggle API Credentials**
   We copy the `kaggle.json` file (you must upload it to your Colab environment first) to the appropriate directory and set the permissions:

   ```python
   !mkdir ~/.kaggle
   !cp kaggle.json ~/.kaggle/
   !chmod 600 ~/.kaggle/kaggle.json
   ```

4. **Download the Dataset from Kaggle**
   Using the Kaggle API, we download the `bitcoin-tweets` dataset:

   ```python
   !kaggle datasets download -d kaushiksuresh147/bitcoin-tweets
   ```

5. **Extract the Dataset**
   Unzips the downloaded file to access the CSV data:

   ```python
   !unzip bitcoin-tweets.zip
   ```

---

###  Data Extraction

We now read and explore the dataset using `cuDF`, the RAPIDS version of Pandas optimized for GPU acceleration.

```python
import cudf
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import matplotlib.dates as mdates

# Read CSV using cuDF
df = cudf.read_csv("/content/Bitcoin_tweets.csv")
```

---

###  Initial Data Exploration 

* **Preview the dataset:**
  Display the first few rows to get a general sense of the data structure.

* **Dataset information:**
  Check data types, memory usage, and missing values using `df.info()`.

* **Summary statistics:**
  Use `df.describe()` to get statistical insights such as mean, standard deviation, min/max values for numeric columns.


---

###  Data Cleaning Summary

The dataset undergoes multiple cleaning steps to ensure consistency, convert types properly, and prepare for analysis:

---

####  Memory Optimization

* Calculated initial and final DataFrame size in **GB** to monitor memory usage improvements.

####  Numeric Conversion

* Converted the following columns to numeric:

  * `'user_followers'`, `'user_friends'`, and `'user_favourites'`
  * Non-numeric values were coerced to `NaN`.
  * Converted final types to `float32` or `uint32`.

####  Textual Cleanup

* Cleaned text-based fields like `'text'` and `'user_description'` by:

  * Lowercasing all text
  * Removing URLs, mentions (@), hashtags (#), special characters, extra spaces, and HTML entities.

####  Boolean Fixes

* Converted `'user_verified'` column to proper boolean format (`True/False`) from strings like `"True"` or integers `1/0`.

####  Missing Value Handling

* Filled missing values in:

  * `'user_description'`, `'user_location'` → with mode or empty string
  * `'hashtags'` → with empty string
  * `'source'` → with `"Unknown Source"`
* Checked for sparse data (over 50% missing) to consider converting to sparse format.

####  Categorical Conversion

* Converted `'user_location'` and `'source'` to **categorical** types if unique values < 50% of total rows.

####  Date Formatting

* Converted `'date'` and `'user_created'` columns from string to proper `datetime` objects using Pandas and cuDF.

####  Final Overview

* Displayed `info()` and `describe()` post-cleaning to summarize structure and statistics.
* Printed final memory usage for comparison.

---

## Bitcoin Tweets Statistical and Probability Analysis

This project performs **descriptive and visual analysis** of Bitcoin-related tweets using statistical and probability concepts. The dataset contains information about tweets, including user metadata such as number of followers, friends, favourites, tweet sources, and verification status.

###  Objective

To understand the temporal patterns and user behaviors behind Bitcoin-related tweets using **annual, monthly, and daily** breakdowns. It includes statistics like average followers, tweet counts, and percentage of verified users over time.

---

###  Steps Performed

#### 1. **Annual Analysis**

* Aggregates and describes user metadata by year.
* Visualizes:

  * Number of tweets per year.
  * Average followers, friends, and favourites per year.
  * Percentage of verified users per year.
  * Top 5 tweet sources per year.

#### 2. **Monthly Analysis**

* Groups data by year and month.
* Visualizes trends similar to annual analysis but at monthly granularity.

#### 3. **Daily Sample Analysis**

* Analyzes a sample week of data from the first year in the dataset.
* Shows:

  * Daily tweet counts.
  * Average followers.
  * Verified user rate per day.

---

###  Visualizations

* All trends are plotted using `seaborn` and `matplotlib`:

  * Line plots for tweet counts and user statistics.
  * Grids and date formatters improve readability.

---

###  Tools and Libraries

* **pandas / cuDF**: Data manipulation
* **seaborn / matplotlib**: Visualization
* **datetime / pandas Periods**: Date processing

---

###  Notes

* The dataset is first converted from cuDF (GPU DataFrame) to pandas for compatibility.
* Descriptive statistics (`.describe()`) are used to show data distributions.
* Sources are optionally plotted if available.

---

###  Example Use Cases

* Social media trend analysis
* Bitcoin market sentiment over time
* User behavior tracking
* Verified vs non-verified user impact
* 

---

### Bitcoin Tweets Analysis Review
To analyze individuals' influence on the Bitcoin market, metrics such as follower count and account verification are crucial. Users with a high number of followers and verified accounts tend to be more influential. A sudden spike in average followers during a specific period may indicate the involvement of prominent figures. Additionally, a higher percentage of verified users in a given timeframe suggests a stronger potential market impact. Analyzing the tweet source also helps distinguish real users from bots.


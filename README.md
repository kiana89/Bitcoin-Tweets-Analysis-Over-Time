##  Bitcoin Tweets Analysis with RAPIDS & Kaggle

This project focuses on extracting and preparing a dataset of Bitcoin-related tweets for further analysis using **GPU-accelerated computing with RAPIDS cuDF** and visualization tools like **Matplotlib** and **Seaborn**.

###  Installation & Setup

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

### Initial Data Exploration 

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

##  Statistical & Probabilistic Analysis of Bitcoin Tweets

This section performs **temporal**, **numerical**, and **correlation-based** analyses of Bitcoin-related tweets, focusing on user metrics, activity patterns, and visualization of trends.

---

###  1. Annual & Monthly Time Series Analysis

* **Grouped by `year` and `year_month`:**

  * Descriptive statistics for:

    * `user_followers`
    * `user_friends`
    * `user_favourites`
  * Visualized trends using line plots:

    *  Number of tweets
    *  Average followers
    *  Average friends
    *  Average favourites
    *  Percentage of verified users
  * Tracked **top tweet sources** per year using grouped line charts.

---

###  2. Daily Sample Analysis

* Selected a **7-day sample** from the earliest year in the dataset.
* Displayed daily statistics and line charts for:

  * Tweet volume
  * Average followers
  * Verified user rate

---

###  3. Violin & Bar Plot Distribution

* **Violin plots** by:

  * Tweet `date` (monthly)
  * `user_created` year
* **Bar plots** for average:

  * `user_followers`
  * `user_friends`
  * `user_favourites`
  * Aggregated by both **tweet date** and **user creation date**.

---

###  4. Correlation Heatmap

* Extracted **date-related features** from `date` and `user_created`:

  * Year, Month, Day, Weekday, Hour
* Generated a **correlation heatmap** among:

  * User metrics (`followers`, `friends`, `favourites`)
  * Time components of tweet and user creation

---

###  5. Pairwise Distribution (Pair Plot)

* Selected a 1,000-row sample from:

  * All numeric and datetime-derived features
* Used `seaborn.pairplot()` to visualize pairwise relationships.

---

>  **Note:** All the charts generated during this analysis are saved in the file named **`chart_pic.zip`** for easy download and reference.

---

 This analysis helps reveal:

* How user engagement changed over time
* Whether followers or favourites are correlated with post frequency or time
* Patterns in verified user behavior
* Impact of user creation date on tweet activity

---

##  Influential Users in Bitcoin Tweet Trends

Based on data analysis, certain types of Twitter users tend to have a greater impact on Bitcoin-related discussions and potentially on its price movements. Key findings include:

1. **Verified Users**

   * Typically celebrities or official organizations.
   * Their tweets gain more attention and correlate with Bitcoin price volatility.

2. **High-Follower Accounts**

   * Users with over 100K followers significantly influence market sentiment.
   * A positive correlation exists between follower count and tweet engagement.

3. **Older Accounts**

   * Users with accounts created before 2017 are generally more trusted in the crypto community.

4. **Tweet Source (Professional Tools)**

   * Tweets sent from platforms like `Twitter Web App` or `TweetDeck` are often more impactful, likely due to professional or organizational usage.

5. **High Engagement Users**

   * Users with higher average likes and retweets have stronger influence.
   * Indicates higher reach and trust among the crypto audience.

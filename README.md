## Bitcoin Tweets Analysis with Kaggle and Python

This project performs a comprehensive **statistical analysis of Bitcoin-related tweets** using GPU-accelerated libraries like **RAPIDS cuDF**, and data visualization tools such as **Matplotlib** and **Seaborn**.

###  Setup and Data Extraction

* Clones the RAPIDS utility repository and installs required dependencies.
* Loads Bitcoin-related tweet data from [Kaggle dataset](https://www.kaggle.com/datasets/kaushiksuresh147/bitcoin-tweets).
* Uses `cuDF` for GPU-accelerated dataframe operations.

###  Data Cleaning

* Converts string-based numeric columns (`user_followers`, `user_friends`, `user_favourites`) to appropriate types.
* Cleans text columns like `text` and `user_description` by:

  * Removing URLs, mentions, hashtags, special characters.
  * Standardizing text to lowercase.
* Handles missing values in critical columns.
* Converts date fields (`date`, `user_created`) to datetime format.
* Optimizes categorical columns like `source` and `user_location`.

###  Statistical & Temporal Analysis

#### **Annual and Monthly Trends**

* Tracks:

  * Number of tweets
  * Average followers, friends, favourites
  * Percentage of verified users
  * Tweet source distribution
* Visualized using `line plots` and `bar plots`.

#### **Daily Sample Analysis**

* Analyzes a one-week window for daily trends in:

  * Tweet frequency
  * User metrics
  * Verification status

#### **Distribution Analysis**

* Violin plots and bar plots of user metrics across:

  * Tweet date
  * User creation date

#### **Correlation and Pair Plot**

* Calculates correlation matrix between user metrics and time-based features.
* Displays relationships using:

  * Heatmap
  * Pair plot (sampled)

###  Libraries Used

* `cudf` (from RAPIDS)
* `pandas`
* `matplotlib`
* `seaborn`
* `re` (regex for text cleaning)

---

###  How to Run

1. Clone the repo in Google Colab or local machine with GPU.
2. Upload `kaggle.json` for API access.
3. Run the notebook step by step.

---

###  Outcomes

* Understand temporal dynamics of Bitcoin discussions on Twitter.
* Identify patterns in user engagement and profile characteristics.
* Highlight the evolution of influence (via followers, verification, etc.) in the crypto community.

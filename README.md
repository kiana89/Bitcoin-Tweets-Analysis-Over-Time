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

### 🗃 Data Extraction

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

###  Initial Exploration

1. **View the First Few Rows**

   ```python
   df
   ```

2. **Dataset Info (Types, Nulls, etc.)**

   ```python
   df.info()
   ```

3. **Descriptive Statistics**

   ```python
   df.describe()
   ```


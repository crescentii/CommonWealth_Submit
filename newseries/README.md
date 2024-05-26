# Reported Addresses

This is my second time to submit report. Compared to the first series, in this report, I optimized my screening process, modified the data source, and added more fields to prove the witch address. You can get report address here [batches1](address1.csv), [batches2](address2.csv) and [batches3](address3.csv) or the following.

```

```

# Description
The Affinity Propagation algorithm is used to filter suspicious data **five** times, strictly reducing false positive samples, and the filtered cluster address data is highly similar. To reduce the possibility of false positive samples, I also manually checked most addresses in submitted cluster. In order to be sufficiently convincing, evidence of the homogenization operation of each cluster address on the entire chain is also provided.

In the address screening, **special attention is paid to the number of decimal places in funding. Since the handling fee for withdrawing CELO from the Binance is 0.001, and normal users will not be bored enough to enter too many decimal places, So funding transactions with more than 3 decimal places are suspicious.**

# Detailed Methodology & Walkthrough
Affinity Propagation is a clustering algorithm that identifies exemplars by passing "responsibility" and "availability" messages between data points. It automatically determines the number of clusters based on a preference parameter and similarity matrix, without requiring the number of clusters to be specified beforehand. I have tried many cluster analysis methods and found this method to be the most useful and effective.

## Dataset

![0.9-1.1CELO sent by binance from 2023-4-1 to 2024-4-1](0.9-1.1CELO_sent_by_binance.png)

(This figure shows CELOchain transactions sent by Binance and number between 0.9 ~ 1.1)

![0.9-1.1CELO sent by binance from 2023-4-1 to 2024-4-1](0.9-1.1CELO_sent_by_binance.png)

(This figure shows CELOchain transactions sent by Binance and number between 1.9 ~ 2.1)

By monitoring the withdrawal operations from the Binance exchange on the CELO chain(shows above), I found that a large number of abnormal behaviors occurred on the chain from July 24th to July 26th and July 18th to Aug 10th on 2023.(withdrawing ~1CELO or ~2CELO from the exchange, and then conducting layerzero transactions). I set the Origin dataset Filter condition to this time period, the sender's tag is Tether-Binance, the transaction amount is 0.9-1.1CELO or 1.9-2.1, and I got about 15,000 pieces of data.

Origin dataset filter condition: 
- Celochain transaction sent by Tether: Binance(0xf64368...)
- The amount of CELO traded is:
  - dataset1: between 0.9-1.1
  - dataset2: between 1.9-2.1
- Transaction time:
  - dataset1: between July-24-2023 and July-26-2023
  - dataset2: between July-18-2023 and Aug-10-2023

Next, I used the transaction data provided by Layerzero to obtain the _'Number of LZ tx' 'Projects' 'Total LZ Volume $' 'Average LZ Volume $' 'Number of OAPPS' 'Number of Sendchian' 'Number of Chian'_ and EarliestLZdata. The initial dataset is here [initial_dataset1](add_lzdata1.csv), [initial_dataset2](add_lzdata2.csv) and [initial_dataset3](dataset3.csv).

Note：the multichain wallet balance$(from debank api) snapshot time: 
- dataset1: around May-24-2024 11:30PM UTC+8
- dataset2: around May-26-2024 1:30PM UTC+8

## Data processing
In the data processing, I used the affinity propation algorithm **five** times to identify the data. After each identification, clusters with less than 20 samples were eliminated, and each identification and filtering process had different goals.

### Genesis filter
This action is to filter addresses that have been marked as witches.

### First filter(initial filter)
The set Affinity Propagation feature parameters are
```python
features = ['Number of LZ tx', 'Average LZ Volume $', 'Number of OAPPS', 'Number of Chian']
```
After obtaining the clustering results, if the number of a certain cluster is less than 20, the cluster will be deleted. This step resulted in the deletion of addresses that had not traded layerzero and real-person accounts that had no problems at all.

### Second filter
The set Affinity Propagation feature parameters are
```python
features = ['Number of LZ tx', 'Total LZ Volume $', 'Average LZ Volume $', 'Number of OAPPS', 'Number of Sendchian','Number of Chian']
```
This is the first comprehensive cluster analysis. After the analysis, clusters with less than 20 counts are again eliminated.

### Third filter
The set Affinity Propagation feature parameters are
```python
features = ['Number of Sendchian','Number of Chian']
```
This clustering analysis reduces the dimensionality of the input parameters.

### Fourth filter(final filter)
The set Affinity Propagation feature parameters are
```python
features = ['Funding block','Funding Amount[CELO]','Number of LZ tx','Total LZ Volume $','Average LZ Volume $','Number of OAPPS','Number of Sendchian','Number of Chian']
```
The input of the last clustering algorithm is intial all available parameters of the initial dataset, and it attempts to divide the dataset into as many clusters as possible. Similarly, clusters with less than 20 internal members will be deleted. This way we get the final output data.
After 4 times of cluster filtering, we now have preliminary processing results, which are [initial_output1](initial_output1.csv), [initial_output2](initial_output2.csv) and [initial_output3](initial_output3.csv). 

### Fifth filter(universal filter)
Here we will show a big killer: multichain wallet balance. In the results obtained after the above 4 times of filtering, I obtained the multichain wallet balances of all addresses in the results through Debank, and then performed a full-data clustering filter, which will greatly reduce the probability of false positive samples. You can get final dataset here: [final_dataset1](dataset1.csv), [final_dataset2](dataset2.csv) and [final_dataset3](dataset3.csv).

The set Affinity Propagation feature parameters are
```python
features = ['Funding block','Funding Amount[CELO]','Number of LZ tx','Total LZ Volume $','Average LZ Volume $','Number of OAPPS','Number of Sendchian','Number of Chian', 'Onmichain Balance$','Onmichain Balance$']
```

After five times identify and filter, we have the final data, see [processed_data1](final_output1.csv), [processed_data2](final_output2.csv) and [processed_data3](final_output3.csv). 

### More ways to remove false positive samples
To provide further evidence, I will provide each cluster address with a large-scale homogenization operation different from  filter source in table (such as a large-scale homogenization withdrawal from the exchange again)
When verifying the evidence I provided, I used random sampling to verify, and paid special attention to the data at the edge of the cluster. Moreover, among the submitted clusters, I mannully excluded clusters whose features did not appear to be particularly obvious. The final cluster had almost no possibility of false positives.

---
## Sybil address batches and extra evidence

---
**batch1**
```
0x0d4b771e25aa007366135e66e38ba02151d0257b

```

**Extra evidence**

In addition to accepting CELO in batches on the CELO chain on June 19th. **This batch of addresses also accepted approximately 0.3AVAX(3$) sent by OKX** at around Sep-7-2023 8:00:11 PM +UTC. Same fund allocation for Omnichain wallets. 

**Data at a glance**
![batch1-1](image1/batch1.png)
![batch1-1-2](image1/batch1-2.png)

---



# Reward Address (If Eligible)
0xfa8d0fc1c3fc17396ace8758079b42ec2dc7bd79

# Appendix
**Affinity propation code and filter**
```python

import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import AffinityPropagation
from sklearn.metrics import pairwise_distances_argmin
import pandas as pd
from sklearn.preprocessing import StandardScaler

def affinity(features, df1):
    X = df1[features].values
    scaler = StandardScaler()
    X = scaler.fit_transform(X)

    af = AffinityPropagation(damping=0.8).fit(X)
    cluster_centers_indices = af.cluster_centers_indices_
    labels = af.labels_

    n_clusters_ = len(cluster_centers_indices)
    print(f"Estimated number of clusters: {n_clusters_}")

    df1['affinity'] = labels
    return df1

def filter(df2):
    affinity_counts = df2['affinity'].value_counts()
    valid_affinity_values = affinity_counts[affinity_counts >= 20].index
    filtered_df = df2[df2['affinity'].isin(valid_affinity_values)]
    return filtered_df

csv_file = 'dataset.csv'
output_file = 'final_aff_result.csv'
df = pd.read_csv(csv_file)
df = df.fillna(0)

features1 = ['Number of LZ tx', 'Average LZ Volume $', 'Number of OAPPS', 'Number of Chian', 'Onmichain Balance$']
df = affinity(features1, df)
df = filter(df)

features2 = ['Number of LZ tx', 'Total LZ Volume $', 'Average LZ Volume $', 'Number of OAPPS', 'Number of Sendchian','Number of Chian', 'Onmichain Balance$']
df = affinity(features2, df)
df = filter(df)

features3 = ['Number of Sendchian','Number of Chian', 'Onmichain Balance$']
df = affinity(features3, df)
df = filter(df)

features4 = ['Funding block','Funding Amount[CELO]','Number of LZ tx','Total LZ Volume $','Average LZ Volume $','Number of OAPPS','Number of Sendchian','Number of Chian', 'Onmichain Balance$','Onmichain Balance$']
df = affinity(features4, df)
df = filter(df)

df.to_csv('final_output',index=False)
```

**A schematic diagram of one result of affinity propagation algorithm**

The z-axis is the multichainwallet balance, and all axis data have been standardized.

![affinity propagation](affinity_propagation.png)

**Dune SQL**
```SQL
SELECT
  date_trunc('day', block_time) AS time,
  COUNT(*) AS transaction_count,
  COUNT(DISTINCT "to") AS receiving_addresses
FROM celo.transactions
WHERE
  block_time BETWEEN TRY_CAST('2023-4-1 00:00:00.000 UTC' AS TIMESTAMP) AND TRY_CAST('2024-4-1 23:59:59.999 UTC' AS TIMESTAMP)
  AND "from" = FROM_HEX('f6436829Cf96EA0f8BC49d300c536FCC4f84C4ED')
  AND value BETWEEN TRY_CAST(0.9 * 1e18 AS DECIMAL(38, 0)) AND TRY_CAST(1.1 * 1e18 AS DECIMAL(38, 0))
GROUP BY 1
ORDER BY
  1 ASC
```






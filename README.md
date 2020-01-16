# SWIFT-MT-940
A high level wrapper of mt-940 package for processing mt-940 swift data into Pandas Dataframe,

The output dataframe contains column of transaction_reference,account_no,statement_number, sequence_number, open_balance_amount,open_balance_date, currency, close_balance_amount, close_balance_date.

## example 
```python
from mt940df import SWIFT
fir_dir = 'Desktop/swiftmt940.txt'

sess = SWIFT(fir_dir)
df = sess.get_clean_df()


```

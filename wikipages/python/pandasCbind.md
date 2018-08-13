---
layout: default
---


If you're doing it on some other columns:

    # Split delimited values in a DataFrame column into two new columns
    df['new_col1'], df['new_col2'] = zip(*df['original_col'].apply(lambda x: x.split(': ', 1)))


If you're doing it on the index:

    nonzero['sample'], nonzero['strategy'] = zip(*pd.Series(nonzero.index).apply(lambda x: x.split('.', 1)))




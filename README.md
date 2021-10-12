# Data 512 A2
By: Andrew Zhou

# Data Sources
This repository uses two data sources.

1) The Wikipedia politicians by country dataset can be found on [Figshare](https://figshare.com/articles/dataset/Untitled_Item/5513449). Download and unzip it to extract the data file, which is called page_data.csv. You can also find the file in my repo at [data/page_data.csv](https://github.com/azhou5211/data-512-a2/blob/main/data/page_data.csv).  
Notes:
In the case of page_data.csv, the dataset contains some page names that start with the string "Template:". These pages are not Wikipedia articles, and should not be included in your analysis.


2) The population data is available in CSV format as WPDS_2020_data.csv. This dataset is drawn from the world population data sheet published by the [Population Reference Bureau](https://www.prb.org/international/indicator/population/table/). You can also find the file in my repo at [data/WPDS_2020_data - WPDS_2020_data.csv](https://github.com/azhou5211/data-512-a2/blob/main/data/WPDS_2020_data%20-%20WPDS_2020_data.csv).   
Notes:
Similarly, WPDS_2020_data.csv contains some rows that provide cumulative regional population counts, rather than country-level counts. These rows are distinguished by having ALL CAPS values in the 'geography' field (e.g. AFRICA, OCEANIA). These rows won't match the country values in page_data.csv, but you will want to retain them (either in the original file, or a separate file) so that you can report coverage and quality by region in the analysis section.

# Article Quality
I obtained article quality in the Wikipedia politicians by country dataset by using a machine learning system called ORES. The article quality estimates are, from best to worst:  

FA - Featured article  
GA - Good article  
B - B-class article  
C - C-class article  
Start - Start-class article  
Stub - Stub-class article  

You can obtain these values by using ORES REST API endpoint. You should review the ORES REST [documentation](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model). It expects a revision ID, which is the third column in the Wikipedia dataset, a model, which is "articlequality", and context which is "enwiki"

Note: It's possible that you will be unable to get a score for a particular article. You can find a list of articles that did not get a score for me at [notebooks/Non-Predicted ORES pages.csv](https://github.com/azhou5211/data-512-a2/blob/main/notebooks/Non-Predicted%20ORES%20pages.csv).

# Combining the Datasets (Data Processing)
You can find my final dataset in [output_files/wp_wpds_politicians_by_country.csv](https://github.com/azhou5211/data-512-a2/blob/main/output_files/wp_wpds_politicians_by_country.csv).

Merge the wikipedia data and population data together. Both have fields containing country names for just that purpose. After merging the data, you'll invariably run into entries which cannot be merged. Either the population dataset does not have an entry for the equivalent Wikipedia country, or vise versa.

Please remove any rows that do not have matching data, and output them to a CSV file called:
```wp_wpds_countries-no_match.csv```  
Consolidate the remaining data into a single CSV file called:
```wp_wpds_politicians_by_country.csv```

The final dataset should have the schema:
Column        | Value
------------- | -------------
country          | Country of the article
article_name         | Name of the article
revision_id         | Revision ID of the article
article_quality_est.         | Article quality predicted by ORES
population         | Population of the country

Notes:  
- article_quality_est. is obtained from the ORES REST API endpoint.  
- population is obtained from world population data sheet published by the [Population Reference Bureau](https://www.prb.org/international/indicator/population/table/).  
- country, article_name, and revision_id are obtained from The Wikipedia politicians by country dataset can be found on [Figshare](https://figshare.com/articles/dataset/Untitled_Item/5513449).

# Notebook
You can find my notebook [here](https://github.com/azhou5211/data-512-a2/blob/main/notebooks/hcds-a2-bias.ipynb). The notebook contains all my methodology for cleaning, processing, merging, and analysis.

# Reflection
- What biases did you expect to find in the data (before you started working with it), and why?    

One of the first things I took notice was the fact that we were using the English Wikipedia. There were so many other wikipedia options, and just by simply choosing the English Wikipedia, we could be introducing unintended bias. It is possible there are a lot of articles that are not in English, since a lot of other countries are likely to use a different language other than English. It is also possible the article quality may be severly skewed due to some articles not having proper english to them, as English is not always a native language to a lot of people. 
- What (potential) sources of bias did you discover in the course of your data processing and analysis?    

It was weird for me that we were obtaining article quality through a trained machine learning algorithm. I did not look at machine learning algorithm in depth, but I would like to know how the model classifies article quality. Is there perhaps a better method of obtaining article quality then relying on a machine learning algorithm? There could be bias trained in the machine learning algorithm, and therefore the article quality given by ORES could be bias.
- Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might create biased or misleading results, due to the inherent gaps and limitations of the data?    

I can see that if we used the data without serious modifications to remove sources of bias and limitations, a model that is trained on this data would be extremely bias. If someone was trying to analyze countries with higher proportions of quality articles, the ORES quality and the English wikipedia are likely going to give them skewed results. Their conclusion of which countries produce higher proportions of quality articles would likely be extremely biased and misleading. Another issue, I can see is the proportion of high quality papers to population. This proportion is strongly skewed as well, since there are some countries such as India and China that have extremely high population. Those proportions will make those countries extremely low, and likely not a good metric to be used.

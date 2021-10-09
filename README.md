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
- What (potential) sources of bias did you discover in the course of your data processing and analysis?
- What might your results suggest about (English) Wikipedia as a data source?
- What might your results suggest about the internet and global society in general?
- Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might create biased or misleading results, due to the inherent gaps and limitations of the data?
- Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might still be appropriate and useful, despite its inherent limitations and biases?
- How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed?

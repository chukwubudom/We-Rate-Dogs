# We-Rate-Dogs

### Gather

Data for the analysis where gotten from three sources by programmatic and manual downloads.

1. The ``twitter_archive_enhanced.csv`` was downloaded manually from the link provided, saved in the working directory and loaded into the notebook.

2. Using ``Requests`` library, the image predictions data was downloaded and loaded into the jupyter notebook.

3. With a developer account, the Twitter API was queried to gather additional data from WeRateDogs twitter archive. The JSON file was saved in a .txt file and read into a pandas dataframe to complete the data gathering process.

### Assess

After the WeRateDogs tweets data were gathered from all the sources provided, visual and programmatic assessments were carried out and the following data quality and tidiness issues were identified.

**Quality Issues**

``df_archive`` table:

* Some ratings in the ``rating_denominator`` column are not equal to 10
* Certain ratings in ``rating_numerator`` column that are decimals were not properly captured
* The table contained retweeted images
* ``tweet_id`` column is integer datatype instead of string
* Variable names ``doggo``, ``floofer``, ``pupper`` and ``puppo`` used as column names
* ``timestamp`` column has the wrong datatype

``df_extra_archive`` table:

* ``id`` column name does not match ``id`` column names in ``df_archive`` and ``df_images`` tables
* `` id`` column is integer datatype instead of string
* There are retweets present

``df_images`` table:

* Duplicates in the ``jpg_url`` column
* ``tweet_id`` column is integer datatype instead of string

**Tidiness Issues**

* Unnecessary columns not required for analysis in all three tables
* Dog breeds and image prediction confidence in nine columns
* The three tables should be in one

### Clean

To initiate the cleaning process, we made a copy of the three dataframes that have been read into the notebook, then took the following steps to clean the data to an acceptable standard for our analysis.

1. Filtered the rating denominators that are not equal to 10 using each tweet’s id, then assigned them the right denominators and numerators. The assigned ratings were determined during the assessment stage. We also deleted some of the denominators that resulted from comments or those that were retweets. The justification for investigating the denominators came from information provided in the project motivation, which mentioned that the denominators are “almost always” equal to 10.

2. Like the rating denominators, we filtered and assigned the correct numerator values in places where numerators are decimals but were not captured right. Before that was done, we changed the datatype of the rating_numerator and ``rating_denominator`` columns to floats, so that they can accept decimals.

3. To remove the retweets, the ``df_archive`` dataframe was filtered and returned only data corresponding to the nulls in ``retweeted_status_user_id`` column. This ensured that all rows corresponding to retweeted data were dropped.

4. Using the ``.apply()`` method,  ``tweet_id`` columns in ``df_archive`` and ``df_extra_archive`` dataframes were converted from integer to string types because they are identifiers and not to be used for any calculation.

5. Variable names should not be used as column names, to correct this problem in our data we used the ``pd.melt`` function to rearrange the ``doggo``, ``floofer``, ``pupper`` and ``puppo`` columns into the ``dog_stage`` column.

6. The ``timestamp`` column containing time and date information was changed from string to datetime datatype. We also changed the format to date only, to show only the dates the tweets were posted.

7. The ``id`` column name in ``df_extra_archive`` table was changed to ``tweet_id``, to correspond to the name of similar column in the other two dataframes. This cleaning will ensure we are able to merge the tables at a later stage. The datatype was also changed to string.

8. Filtering rows corresponding to the null values in ``retweeted_status`` column of ``df_extra_archive``, all retweets in the table were dropped.

9. The duplicate values in the ``df_images`` table were dropped.

The above steps concluded the cleaning efforts for the quality issues.

Next the tidiness issues identified during assessment were similarly cleaned as follows:

1. Across the three dataframes, all unnecessary columns to the analysis were dropped, leaving the important columns for analysis and other that makes our data more complete.

2. Using a nested-if function the information in 9 columns pertaining to dog image predictions were reduced to only two ``dog_breed`` and ``prediction``. Then the other columns holding the date initially were dropped.

3. After the cleaning, all three dataframes were merged into one using ``pandas.merge`` function. Then the columns were reordered and the rows sorted by date. Finally, the data was saved as ``twitter_archive_master.csv``. 

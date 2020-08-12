## Country extractor from News Articles

This program relies on [geotext, ](https://pypi.org/project/geotext/) [flashgeotext](https://flashgeotext.iwpnd.pw/) and the [Stanford Named Entity Recognizer (NER)](https://nlp.stanford.edu/software/CRF-NER.shtml) to predict the [2-letter country code](https://www.iban.com/country-codes) from the text of an English news article. It also gives a list of all countries, cities, states, and nationalities found in that article.

The country extractor program has several advantages over other libraries that do the same task, such as flashgeotext and spaCy. These are:
1. Adding [plural versions](https://en.wikipedia.org/wiki/List_of_adjectival_and_demonymic_forms_for_countries_and_nations) of every country (for example 'Chinese', 'American', 'Indian', etc). This feature is not present in either flashgeotext or spaCy, and leads to many country mentions being ignored.
2. Removing approximately 300 words that were incorrectly being labelled as cities (such as 'Buy', 'Sulphur', 'Sunset', etc.) from the demo data of flashgeotext.
3. Adding more than 200 global cities and 2500 Indian cities that were either not present in flashgeotext's demo data, or were being misclassified. Examples of these include 'Nigeria' and 'Frankfurt' (that weren't being labelled at all by flashgeotext), and 'Kandahar' and 'Jalalabad' (that were being labelled in India instead of Afghanistan). This step not only increases the program's performance on Indian news articles, but also also leads to more locations being predicted overall compared to spaCy and flashgeotext.
4. Adding [US](https://www.ncbi.nlm.nih.gov/books/NBK7254/) and [Canadian](https://en.wikipedia.org/wiki/List_of_U.S._state_abbreviations) state names, along with their abbreviations (for example: 'california', 'calif.', 'FL.', 'B.C.' etc.). 
5. Adding weights of 17, 10, 7, 5 and 2 to those locations that are present in the first 20, 100, 250, 1000 and >1000 characters of the article's text. The intuition behind this step is that the article's location is usually found at the beginning of the text.
6. Adding weights of 25, 20 and 15 for those cities/countries that are preceded by an 'in' (for example, 'in Moscow', 'in Syria') and which are present in the first 25, 100 and 250 characters of the article, respectively.
7. Writing functions to easily add new cities that are not present in the demo data of flashgeotext, as well as adding the ability to remove words that should be ignored.
8. Using Stanford's NER to remove the reporter's name from the article text. This creates a problem when the reporter's name includes words that are counted as a city. For example, 'Troy Greenwood' would be called a city due to 'Troy'. Approximately 500 such names have been added to the list of words that should be ignored while making predictions.
9. Adding 32 countries (along with their capitals) that were not being predicted at all in the vanilla version of flashgeotext.
10. Adding 'UN' and 'EU' as part of the output, so as to include those articles that are from the United Nations or the European Union.

### How to use this program:
1. Clone the master branch of this repository and open the 'country extractor.ipynb' file in Jupyter Notebooks.
2. Create an excel file with a 'text' column and place the text of the news articles in it. Save this file and point df_full to this file (in the 5th block of the iPython notebook).
3. Scroll to the end of the notebook where it says 'df.to_excel' and specify where you want the output to be saved. It will be exported to an excel file by default, but this can be changed to .csv or any other format based on the user's preference.
4. If you do not want to manually add/modify the list of cities that are detected by the program you can run all the cells and find your output in the place you had set earlier, once the program finishes running.

### How to add or modify a country or city:
1. Scroll to the section where addCity1 is being called.
2. Add the city with the following code:
addCity1(['nameOfCity1', 'nameOfCity2'], 'XYabcd', special=0) where XY is the 2 letter country code of the city, and abcd is any alphanumeric string that can be used to separate that entry from other cities that have already been added. 

If the city or country being added is a special name, for example an abbreviation (like 'U.K.') or a government body's name (like 'Fed'), pass special=1 to addCity1 so that the createUpperAndLowerCase() function is not called on those names. This will prevent lowercase versions of the city from being recognised in the text. For example, if you added 'Obama' to US then 'obama' would be recognised if special=0 was passed, but would not be recognised if special=1 was passed.
3. Scroll to the section where addCity2 is being called and add addCity2(['nameOfCity1', 'nameOfCity2'], 'XYabcd'). The special=0 or 1 parameter does not need to be passed here.
Note: The characters after the 2 letter country code are ignored while calculating the final count of countries mentioned in the news article, so 'IN25' and 'IN27' would both result in 'IN' being added to the final list of countries mentioned. I would recommend starting 'abcd' from 30 onwards, for example using 'IN30', 'IN31', 'US31', 'CN31', since most of the values below 20 have been used by me.

### How to add or modify words to be ignored by flashgeotext:
1. Scroll to the 'delete_these_words' list and add the word you wish to ignore to it. It will then not be classified as a city when the program is re-run.


### Versions of dependent libraries as of August 12, 2020
1. geotext: v 0.4.0
2. flashgeotext: v 0.2.0
3. Stanford NER (not required to run 'country extractor.ipynb'): v 4.0.0

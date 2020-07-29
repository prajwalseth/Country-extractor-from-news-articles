## Country extractor from News Articles

This module relies on the [flashgeotext](https://flashgeotext.iwpnd.pw/) library and the [Stanford Named Entity Recognizer (NER)](https://nlp.stanford.edu/software/CRF-NER.shtml) to predict the [2-letter country code](https://www.iban.com/country-codes) of an English news article. It also gives a list of all countries, cities, states, and nationalities found in that article.

The country extractor library has several advantages over other libraries that do the same task, such as flashgeotext and spaCy. These are:
1. Adding [plural versions](https://en.wikipedia.org/wiki/List_of_adjectival_and_demonymic_forms_for_countries_and_nations) of every country (for example 'Chinese', 'American', 'Indian', etc). This feature is not present in either flashgeotext or spaCy, and leads to many country mentions being ignored.
2. Removing approximately 300 words that were incorrectly being labelled as cities (such as 'Buy', 'Sulphur', 'Sunset', etc.) from the demo data of flashgeotext. This significantly improves the library's accuracy, as these words were otherwise being included in the list of predicted locations from flashgeotext.
3. Adding more than 150 global cities and 2500 Indian cities that were either not present in flashgeotext's demo data, or were being misclassified. Examples of these include 'Nigeria' and 'Frankfurt' (that weren't being labelled at all by flashgeotext), and 'Kandahar' and 'Jalalabad' (that were being labelled in India instead of Afghanistan). This step not only increases my library's performance on Indian news articles, but also also leads to more locations being predicted overall compared to spaCy and flashgeotext.
4. Adding [US](https://www.ncbi.nlm.nih.gov/books/NBK7254/) and [Canadian](https://en.wikipedia.org/wiki/List_of_U.S._state_abbreviations) state names, along with their abbreviations (for example: 'california', 'calif.', 'FL.', 'B.C.' etc.). 
5. Adding weights of 17, 10, 7, 5 and 2 to those locations that are present in the first 20, 100, 250, 1000 and >1000 characters of the article's text. The intuition behind this step is that the article's location is usually found at the beginning of the text.
6. Adding weights of 25, 20 and 15 for those cities/countries that are preceded by an 'in' (for example, 'in Moscow', 'in Syria') and which are present in the first 25, 100 and 250 characters of the article, respectively.
7. Writing functions to easily add new cities that are not present in the demo data of flashgeotext, as well as adding the ability to remove words that should be ignored.
8. Using Stanford's NER to remove the reporter's name from the article text. This creates a problem when the reporter's name includes words that are counted as a city. For example, 'Troy Greenwood' would be called a city due to 'Troy'. Approximately 500 such names have been added to the list of words that should be ignored while making predictions.
9. Adding 32 countries (along with their capitals) that were not being predicted at all in the vanilla version of flashgeotext.
10. Adding 'UN' and 'EU' as part of the output, so as to include those articles that are from the United Nations or the European Union.

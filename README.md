This module relies on the [flashgeotext](https://flashgeotext.iwpnd.pw/) library and the [Stanford Named Entity Recognizer (NER)](https://nlp.stanford.edu/software/CRF-NER.shtml) to return the [2-letter country code](https://www.iban.com/country-codes) of the country that the news article (written in English) is talking about. It also gives a list of all countries, cities, states, and nationalities found in the article.

My approach has several advantages over the vanilla flashgeotext library. These are:
⋅⋅* Adding [plural versions](https://en.wikipedia.org/wiki/List_of_adjectival_and_demonymic_forms_for_countries_and_nations) of every country (for example 'Chinese', 'American', 'Indian', etc.) 
⋅⋅* Removing approximately 800 words that were incorrectly being labelled as cities by flashgeotext (such as 'Buy', 'Sulphur', 'Sunset', etc.)
⋅⋅* Adding more than 100 global cities and 2500 Indian cities that were either not present in flashgeotext's demo data, or were being mislabelled as being from another country. For example, 'Nigeria' and 'Frankfurt' weren't being labelled at all, and Kandahar and Jalalabad were being shown in India instead of Afghanistan.
⋅⋅* Adding Indian, [US](https://www.ncbi.nlm.nih.gov/books/NBK7254/), and [Canadian](https://en.wikipedia.org/wiki/List_of_U.S._state_abbreviations) state names, along with their abbreviations (for example: 'california', 'calif.', 'FL.', etc.).
⋅⋅* Adding weights of 17, 10, 7, 5 and 2 to those locations that are present in the first 20, 100, 250, 1000 and >1000 characters of the article's text.
	Adding more words to the list of deleted words (an example of this is words like 'Of' that were incorrectly labelled as being locations)
⋅⋅* Writing functions to easily add new cities that are not present in flashgeotext's demo data.
⋅⋅* Adding weights of 25, 20 and 15 for those cities/countries that are preceded by an 'in' (for example, 'in Moscow', 'in Syria') and which are present in the first 25, 100 and 250 characters of the article, respectively.
⋅⋅* Using Stanford NER to remove the reporter's name from the article text. This creates a problem when the reporter's name includes words that are counted as a city. For example, 'Troy Greenwood' would be called a city due to 'Troy'.
⋅⋅* Adding 32 countries (along with their capitals) that were not being predicted at all in the vanilla version of flashgeotext.

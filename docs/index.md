# Efficient Vietnamese Name Retrieval using Highly Discriminative N-Grams

## Introduction

According to Wikipedia, there are more than [5 millions](https://en.wikipedia.org/wiki/Overseas_Vietnamese) overseas Vietnamese in the world. Detect Vietnamese names from big global database is useful for some applications and community activities.

Recognizing Vietnamese names poses a challenge due to their shared Latin characters and similar word forms with names from other countries. For instance, the name “Nguyen” (folding form of popular surname Nguyễn or name Nguyên) is easily identifiable as Vietnamese. However, widespread Vietnamese surnames like “Lê” when written without accents, can be confused with common French prepositions like “Le” and “Đỗ” may be mistaken for the English verb “Do”. Furthermore, some names, such as “Vân” or “Văn”, sound distinctly Vietnamese but, when written without diacritics, can be confused with the common Dutch name “Van”.

This research shows the statictics of Vietnamese name based on a big dataset and propose a set of Highly Discriminative Names to retrieve Vietnamese names with high precisiona with recall from any global name database.

## Dataset

The research dataset consists of positive and negative examples of Vietnamese names gathered from various sources: 

* names extracted from the Vietnamese phonebook through web crawling,
* student names from several universities in Vietnam,
* names extracted from a European country’s phonebook via web crawling
* names obtained from Wikipedia,
* names sourced from LinkedIn profiles. 

All names are normalized, which involves removing accents. For example, “Trần Văn” is transformed into “tran van”. Single-character tokens are excluded to reduce the risk of errors with abbreviations.

**Table 1. Overview of name dataset used in the research** 

| Total names | 4,126,815 |
|:---------------- | ----:|
| Positive names (Vietnamese names) | 635,678 |
| Negative names (Non-Vietnamese names) | 3,491,137 |
| Total tokens | 9,423,808 |
| Average tokens per name | 2.38
| Unique unigrams | 276,890 |
| Positive unigrams | 4,099 |

The top 10 unigrams and bigrams from Vietnamese names are presented in **Table 2**, revealing that a few tokens dominate the majority of names. Approximately 4,000 unigrams constitute over 635,000 Vietnamese names, with 124 of them classified as highly discriminative (primarily appearing in Vietnamese names but rarely in names from other nations). These 124 discriminative names encompass over 84% of Vietnamese names in our dataset, with “nguyen” being the most prevalent, accounting for nearly 9%. 

**Table 2. Top 10 unigrams from ~635K Vietnamese names**

| Unigram |  Percentage |
|:---------------- | ----:|
| nguyen | 8.90 |
| thi | 7.90 |
| van | 3.74 | 
| tran | 2.97 |
| le | 2.83 | 
| hoang | 2.27 | 
| thanh | 2.11 | 
| ngoc | 2.06 | 
| pham | 2.01 | 
| anh | 1.82 |

If we use all 4000 unigrams as the query to retrieve Vietnamese names obviously we can reach to 100% recall. However the precision of result set is very low. The experiment from out dataset showed that when using nearly 1500 unigrams for query the precision already drops to 60% (Figure 1). Easyly see that among top 10 popular Vietnamese unigram, "le" and "van" certainly retrieve many fall positive cases which are European names.

**Figure 1. Precision and Recall (%) when using only unigram for retrieval**
![PrecRecallUnigrams](https://github.com/user-attachments/assets/908273c1-6ae2-446d-a4b4-1befe4254aba)

To increase the recall and keep high precision (prefer precision at 99.9% so that from a result set of 10'000 names, ~10 names are false positive), we introduce new method using Highly Discriminative N-Grams for Vietnamese name retrieval.

## Highly Discriminative N-Grams

From what we observe in our corpus of Vietnamese and international names, there are sets of tokens that appear only in Vietnamese names. For example “nguyen”, “huyen”, “phuong” are rarely found in the person’s name in other nations. They are considered highly discriminative unigrams. On the other hand, some unigrams, which are non-discriminative, and are common in both Vietnamese and foreign names, could be combined to create new highly discriminative bigrams. 

For example “tran” and “van” are non-discriminative unigrams as they appear in other nationalities, however, when they are combined, this pair becomes a discriminative feature for the Vietnamese name “tran van”. This bigram “tran van” is a commonly occurring combination, typically representing the family name “Trần” along with the middle or first name “Văn” in Vietnam.

A token or pair of tokens defined as *Higly Discriminative N-Gram (HDN)* if they satisfy the following criteria:

* the _frequency_ appearing in positive names is bigger than a threshold. This threshold is used to filter out some spelling mistakes form of tokens in the training set or abnormal names. In the beginning, we set threshold to 10. Higher this value, more confident the estimated probability.
* the _confidence score_ is bigger than a threshold which is set every high (e.g. 0.995).

The confidence score is calculated based on the estimated probability of a token being a Vietnamese name, derived from the dataset. However, we enhance the negative cases of tokens using a function to ensure that tokens frequently appearing in negative label names receive a very low confidence score. This negative amplification ensures that if a token is highly prevalent in Vietnamese names but is not rare in negative names, it is not considered highly discriminative. For example, name "le", appear in 180 negative names, which is only 0.3% of total 58152 names containing "le", but confidence score is 0.75, then it is not Highly Discriminative Unigram.

Top 10 bigrams and its percentage  are showed in *Table 3*. It's worth noting that the most popular bigram “nguyen thi” is not included here because we generate bigrams only when the unigram is not a discriminative token. Since “nguyen” is a discriminative unigram for Vietnamese names, all bigrams containing it are not generated. This approach helps store the number of highly discriminative n-grams small for the efficient classifier in practice. In fact, the total generated bigrams are only 37,914 and 8,476 among them are discriminative which is simple to manage.

**Table 3. Top 10 bigrams from ~635K Vietnamese names**

| Bigram |  Percentage |
|:---------------- | ----:|
| tran van | 0.99
| le van | 0.88 | 
| van vu | 0.45 | 
| bui van | 0.34 |
| do van | 0.34 | 
| duc tran | 0.30 | 
| hong le | 0.30 | 
| thu tran | 0.30 | 
| dinh van | 0.29 | 
| le van | 0.29 |

Figure 2 shows that after using 100 HD-Unigrams for retrieval (which reach recall at 86%), we start to use HD-Bigrams for the retrieval query on the experiment dataset. The presision of results remain at 99.9%, and the recall increases continuously to 98% with 2300 bigrams. 

Using 500 HDNs for Vietnamese name retrieval we can obtain recall at 95% and precision at 99.9%. If we want to limit number of queries to submit at 100 (this is very practical constrain because some systems limit number of queries submitted), then with 69 HD-Unigrams and 31 HD-Bigrams, we can obtain recall ~90% and precision >99.9%.

**Figure 2. Precision and Recall (%) when using Hightly Discriminative Ngrams for retrieval**
![PrecRecallNGrams](https://github.com/user-attachments/assets/b7515fc2-4d46-471c-a5e1-6e6711f36127)

Difference experiment setup is showed in  *Table 4* can help us to decide on how many ngrams, what are minimum frequency and confidence to choose to prefer precision or recall or number fo queries to use to retrieve Vietnamese names.

**Table 4. Different experiment setup and results**

|  # Unigrams | # Bigrams | Min Frequency | Min Confidence | Precision | Recall | 
|----|----|----|----|----|----|----|
| 232 | 6311 | 10 | 0.90 |  99.762 | 99.072 | 
| 179 | 3229 | 30 | 0.95 |  99.822 | 93.613 | 
| 111 | 2334 | 50 | 0.99 |  99.910 | 98.078 | 
| 111 | 1329 | 100 | 0.99 |  99.919 | 97.437 |
| 100 | 400 | 100 | 0.99 |  99.928 | 95.181 | 
| 69 | 31 | 100 | 0.99 |  99.943 | 89.796 | 

=======
## Appendix

List of  more than 400 Hightly Discriminative Unigrams & Bigram can be [download here](https://github.com/toanluu/vietname/blob/master/data/Top400-HighlyDiscriminative-VietNames.csv). Please cite "Discriminative Vietnamese Names by VietSearch" if you use this dataset for any publication.


NEWS
====

Versioning
----------

Releases will be numbered with the following semantic versioning format:

&lt;major&gt;.&lt;minor&gt;.&lt;patch&gt;

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major (and resets the minor
  and patch)
* New additions without breaking backward compatibility bumps the minor
  (and resets the patch)
* Bug fixes and misc changes bumps the patch



sentimentr 2.3.0 - 
----------------------------------------------------------------

**BUG FIXES**

**NEW FEATURES**

**MINOR FEATURES**

**IMPROVEMENTS**

**CHANGES**


sentimentr 2.1.0 - 2.2.3
----------------------------------------------------------------

**BUG FIXES**

* `sentiment` contained a bug that caused sentences with multiple polarized 
  words and comma/semicolon/colon breaks to inappropriate replicate rows too many
  times (a recycling error).  This in turn caused the same polarized word to be
  counted multiple times resulting in very extreme polarity values.  This was 
  spotted by Lilly Wang.
  
* `validate_sentiment` contained an error in the documentation; the predicted
  and actual data were put into the wrong arguments for the first example.

**NEW FEATURES**

* The default sentiment sentiment lookup table used within **sentimentr** is now
  `lexicon::hash_sentiment_jockers_rinker`, a combined and augmented version of
  `lexicon::hash_sentiment_jockers` (Jockers, 2017) & Rinker's augmented 
  `lexicon::hash_sentiment_huliu` (Hu & Liu, 2004) sentiment lookup tables.

* Five new sentiment scored data sets added: `kaggle_movie_reviews`, `nyt_articles`
  `hotel_reviews`, `crowdflower_self_driving_cars`, `crowdflower_products`,
  `crowdflower_deflategate`, `crowdflower_weather`, & `course_evaluations` for 
  testing nd exploration.

*  `replace_emoji` and `replace_emoji_identifier` rexported from the **textclean**
  package for replacing emojis with word equivalents or an identifier token
  that can be detected by the `lexicon::hash_sentiment_emoji` polarity table 
  within the `sentiment` family of functions.
  
**MINOR FEATURES**

* `sentiment` picks up the `neutral.nonverb.like` argument.  This allows the
  user to treat specific non-verb uses of the word 'like' as neutral since 'like' 
  as a verb is usually when the word is polarized.
  
* `combine_data` added to easily combine trusted **sentimentr** sentiment 
  scored data sets.

**CHANGES**

* The sentiment data sets have been reformatted to conform to one another.  This
  means columns have been renamed, ratings have been rescales to be zero as neutral,
  and columns other than `sentiment` score and `text` have been removed.  This
  makes it easier to compare and combine data sets.

* `update_key` now allows a **data.table** object for `x` meaning **lexicon**
  `hash_sentiment_xxx` polarity tables can be combined.  This is particularly 
  useful for combining `hash_sentiment_emojis` with other polarity tables.


sentimentr 2.0.1
----------------------------------------------------------------

**BUG FIXES**

* `get_sentences` assigned the class to the data.frame when a data.frame was passed but not to the text column, meaning the individual column could not be passed to `sentiment` or `sentiment_by` without having sentence boundary detection re-done.  This has been fixed.  See #53.



sentimentr 1.0.1 - 2.0.0
----------------------------------------------------------------

**BUG FIXES**

* `sentiment_attributes` gave an incorrect count of words.  This has been fixed 
  and number of tokens is reported as well now.  Thanks to Siva Kottapalli for
  catching this (see #42).
  
* `extract_sentiment_terms` did not return positive, negative, and/or neutral
  columns if these terms didn't exist in the data passed to `text.var` making it 
  difficult to use for programming.  Thanks to Siva Kottapalli for
  catching this (see #41).
  
* `rescale_general` would allow `keep.zero` when `lower` &gt;= 0 meaning the 
original mid values were rescaled lower than the lowest values.
  
  
**MINOR FEATURES**

* `validate_sentiment` picks up Mean Directional Accuracy (MDA) and Mean 
  Absolute Rescaled Error (MARE) measures accuracy.  These values are printed 
  for the `validate_sentiment` object and can be accessed via `attributes`.

**CHANGES**

* Many **sentimentr** functions performed sentence splitting (sentence boundary 
  disambiguation) internally.  This made it (1) difficult to maintain the code,
  (2) slowed the functions down and potentially increased overhead memory, and 
  (3) required a repeated cost of splitting the text every time one of these
  functions was called.  Sentence splitting is now handled vie the **textshape**
  package as the backend for `get_sentences`.  It is recommended that the user
  spits their data into sentences prior to using the sentiment functions.  Using
  a raw character vector still works but results in a warning.  While this won't
  break any code it may cause errors and is a fundamental shift in workflow,
  thus the major bump to 2.0.0
  
  
sentimentr 0.5.0 - 1.0.0
----------------------------------------------------------------

**BUG FIXES**

* Previously `update_polarity_table` and `update_valence_shifter_table` were
  accidentally not exported.  This has been corrected.
  
**NEW FEATURES**

* `downweighted_zero_average`, `average_weighted_mixed_sentiment`, and 
  `average_mean` added for use with `sentiment_by` to reweight
  zero and negative values in the group by averaging (depending upon the 
  assumptions the analyst is making).
  
* `general_rescale` added as a means to rescale sentiment scores in a 
  generalized way.
  
* `validate_sentiment` added as a means to assess sentiment model performance
  against known sentiment scores.
  
* `sentiment_attributes` added as a means to assess the rate that sentiment
  attributes (attributes about polarized words and valence shifters) occur and 
  co-occur.

**MINOR FEATURES**

* `sentiment_by` becomes a method function that now accepts `sentiment_by`
  and `sentiment` objects for `text.var` argument in addition to default
  `character`.

**IMPROVEMENTS**

* `sentiment_by` picks up an `averaging.function` argument for performing the 
  group by averaging.  The default uses `downweighted_zero_average`, which 
  downweights zero values in the averaging (making them have less impact).  To
  get the old behavior back use `average_mean` as follows.  There is also an
  `average_weighted_mixed_sentiment` available which upweights negative 
  sentences when the analysts suspects the speaker is likely to surround 
  negatives with positives (mixed) as a polite social convention but still the 
  affective state is negative.

**CHANGES**

* The hash keys `polarity_table`, `valence_shifters_table`, and `sentiword` have
  been moved to the **lexicon** (https://github.com/trinker/lexicon) package in
  order to make them more modular and maintainable.  They have been renamed to
  `hash_sentiment_huliu`, `hash_valence_shifters`, and `hash_sentiment_sentiword`.
  
* The `replace_emoticon`, `replace_grade` and `replace_rating` functions have 
  been moved from **sentimentr** to the **textclean** package as these are 
  cleaning functions.  This makes the functions more modular and generalizable 
  to all types of text cleaning.  These functions are still imported and 
  exported by **sentimentr**.

* `but.weight` argument in `sentiment` function renamed to `adversative.weight`
  to better describe the function with a linguistics term.

* `sentimentr` now uses the Jockers (2017) dictionary by default rather than the 
  Hu & Liu (2004).  This may result in breaks to backwards compatibility,
  hence the major version bump (1.0.0).

sentimentr 0.3.0 - 0.4.0
----------------------------------------------------------------

**BUG FIXES**

* Missing documentation for `but' conjunctions added to the documentation.  
  Spotted by Richard Watson (see #23).
  
**NEW FEATURES**

* `extract_sentiment_terms` added to enable users to extract the sentiment terms 
  from text as `polarity` would return in the **qdap** package.
  
**MINOR FEATURES**

* `update_polarity_table` and `update_valence_shifter_table` added to abstract 
  away thinking about the `comparison` argument to `update_key`.


sentimentr 0.2.0 - 0.2.3
----------------------------------------------------------------

**BUG FIXES**

* Commas were not handled properly in some cases.  This has been fixed (see #7).

* `highlight` parsed sentences differently than the main `sentiment` function 
  resulting in an error when `original.text` was supplied that contained a colon
  or semi-colon.  Spotted by Patrick Carlson (see #2).

**MINOR FEATURES**

* `as_key` and `update_key` now coerce the first column of the `x` argument 
  data.frame to lower case and warn if capital letters are found.

**IMPROVEMENTS**

* A section on creating and updating dictionaries was added to the README:
  https://github.com/trinker/sentimentr#making-and-updating-dictionaries
  
* `plot.sentiment_by` no longer color codes by grouping variables.  This was
  distracting and removed.  A jitter + red average sentiment + boxplot visual
  representation is used.
  
**CHANGES**

* Default sentiment and valence shifters get the following additions: 
  - `polarity_table`: "excessively", 'overly', 'unduly', 'too much', 'too many', 
  'too often', 'i wish', 'too good', 'too high', 'too tough'
  - `valence_shifter_table`: "especially"


sentimentr 0.1.0 - 0.1.3
----------------------------------------------------------------

**BUG FIXES**

* `get_sentences` converted to lower case too early in the regex parsing,
  resulting in missed sentence boundary detection.  This has been corrected.

* `highlight` failed for some occasions when using `original.text` because the
  splitting algorithm for `sentiment` was different. `sentiment`'s split algorithm
  now matches and is more accurate but at the cost of speed.

**NEW FEATURES**

* `emoticons` dictionary added.  This is a simple dataset containing common
  emoticons (adapted from [Popular Emoticon List](http://www.lingo2word.com/lists/emoticon_listH.html))

* `replace_emoticon` function added to replace emoticons with word equivalents.

* `get_sentences2` added to allow for users that may want to get sentences from
  text and retain case and non-sentence boundary periods.  This should be
  preferable in such instances where these features are deemed important to the
  analysis at hand.

* `highlight` added to allow positive/negative text highlighting.

* `cannon_reviews` data set added containing Amazon product reviews for the
  Cannon G3 Camera compiled by Hu and Liu (2004).

* `replace_ratings` function + `ratings` data set added to replace ratings.

* `polarity_table` gets an upgrade with new positive and negative words to
  improve accuracy.

* `valence_shifters_table` picks up a few non-traditional negators.  Full list
  includes: "could have", "would have", "should have", "would be",
  "would suggest", "strongly suggest".

* `is_key` and `update_key` added to test and easily update keys.

* `grades` dictionary added.  This is a simple dataset containing common
  grades and word equivalents.

* `replace_grade` function added to replace grades with word equivalents.


**IMPROVEMENTS**

* `plot.sentiment` now uses `...` to pass parameters to **syuzhet**'s
  `get_transformed_values`.

* `as_key`, `is_key`, & `update_key` all pick up a logical `sentiment` argument
  that allows keys that have character y columns (2nd column).



sentimentr 0.0.1
----------------------------------------------------------------

This package is designed to quickly calculate text polarity sentiment at the
sentence level and optionally aggregate by rows or grouping variable(s).
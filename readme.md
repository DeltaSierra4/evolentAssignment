# Basic usage

You can run the script with the following command:

```
$ python3 csvloader.py <csv dataset> [options]
```

For instructions on what options are available, run the following command:

```
$ python3 csvloader.py -h
```

To run the script to provide answers to questions as requested, the following command will work:

```
$ python3 csvloader.py <csv dataset> -mo -ma -mc -st -si -cr -ra
```

If you want to run the script with different arguments, you can do so. For example, if you want to see the creator with the most Human type, you can use the following command:

```
$ python3 csvloader.py <csv dataset> -cwmst Human
```


# Usage for each question

For the answer to question 1 only, any of the following commands would work:

```
$ python3 csvloader.py <csv dataset> -mo -t -1 -rwmsa "" -cwmst "" -ra -1
$ python3 csvloader.py <csv dataset> -ma -t -1 -rwmsa "" -cwmst "" -ra -1
$ python3 csvloader.py <csv dataset> -mc -t -1 -rwmsa "" -cwmst "" -ra -1
```

For the answer to question 2 only, the following command would work:

```
$ python3 csvloader.py <csv dataset> -rwmsa "" -cwmst "" -ra -1
```

For the answer to question 3 only, the following command would work:

```
$ python3 csvloader.py <csv dataset> -t -1 -cwmst "" -ra -1
```

For the answer to question 4 only, any of the following command would work:

```
$ python3 csvloader.py <csv dataset> -t -1 -rwmsa "" -ra -1
```

For the answer to question 5 only, the following commands would work:

```
$ python3 csvloader.py <csv dataset> -st -si -cr -t -1 -rwmsa "" -cwmst "" -ra -1
```

For the answer to question 8 only, any of the following command would work:

```
$ python3 csvloader.py <csv dataset> -t -1 -rwmsa "" -cwmst ""
```


# Answers to each questions

1. Question 1
The definition of the most powerful superhero can be defined in many different ways. The simplest measure is to simply refer to the overall_score. Another way is to compute the average score of the hero's stats, or simply to obtain a sum of all the stat scores. Another way is to count how many abilities a hero has. These methods are implemented as overall_power(), average_power(), and ability_counter()

2. Question 2
We can tally_abilities() of superheroes to show the top x number of abilities. First, list_abilities() is called to extract the list of abilities in the dataframe, then the function iterates through each ability to tally the total by simply adding the values in the columns representing each ability (thanks to the fact that the columns represent boolean values of whether a superhero has an ability or not). Before the sum is obtained, however, the dataframe selection must be converted to a numeric value using pd.to_numeric() since the values are stored as strings initially.

3. Question 3
We can tally_abilities_by_race() of superheroes to show which race has the most of a given ability. Input check is performed before iterating through all race types (gathered by the list_races() function) and simply adding up the values of a given ability boolean value. Also, as stated in list_races(), any row that is missing the race value is simply referred to as 'Unknown' (instead of using an empty string as recorded in the dataframe).

4. Question 4
We can tally_race_by_creators() of superheroes to show which creator has the most of a given race. Simply search through the pandas dataframe by looking for matching data (in the case of question 4, look for rows where type_race matches the given race type for each creator), then count the number of rows.

5. Question 5
We can tally_teams_by_creators() of superheroes to show which creator has the most superhero teams. Sort the dataframe into subsets by creators, and then combine all team names into sets and tally the number of unique teams from each creator. (If there are no team names included in the dataset at all, then this function will print indicating such). Note that the csv file saved the list of team names as a string, but we want them in an object(list) format, so we use ast.literal_eval. The set of all team names sorted by creators is returned by this function to be used by crossover_check() to answer question 5b in the coding challenge. For this reason, only creators with any number of team names are considered. (i.e. any creator that doesn't have any team name will not be included)

Question 5a
We can find_hero_with_most_team() by going through the dataset by looking only at the columns name, real_name, full_name, and aliases as requested by question 5a. First, preprocess the dataset by removing any rows that don't have any team data at all (and exit the function if there are no rows that contain team data), then look for the row that contains the most number of teams and print the results.

Question 5b
We can crossover_check() between teams from each creator. Simple intersect() call over all creators and their sets of team names.

6. Question 6
This method was not implemented due to lack of time, but there are two possible strategies to find the best predictors of a character's alignment. 
One way is to train a classifier using the character description as a labeled training set. NLP frameworks such as spacy would be used here. A method would be implemented to convert the dataset into a spacy training dataset and run the spacy train command to produce a model that can predict a character's alignment given a text description of the character. The issue with this method is that there are only 1450 rows of data available, which may not be sufficient to train an accurate model. In addition, training a model would consume a long time and may consume large amounts of resources as the training dataset grows in size.
Another way to predict a character's alignment is using decision trees. Stats such as abilities or stat scores can be used to train a decision tree to predict a character's alignment. This may run the risk of overfitting due to the training set's small size - if a new character is introduced that strays significantly from the pattern seen by the decision tree, the prediction may not be accurate.

7. Question 7
This method was not implemented due to lack of time, but it is possible to classify a character's history text as negative either by using an NLP classifier model (such as spacy), use a similarity function to compare the character's history text to a negative story or a positive story, or even use a keyword matching algorithm to find how many negative keywords are found in a character's history text and use a threshold to classify a character's history text as negative. The results of the classification can be attached to the dataframe and then a simple query would show a list of characters who have had negative history but are aligned as Good.

Question 7b
One way to establish patterns in text is through extracting keywords via a clustering algorithm. Python's Textacy framework provides many ways to accomplish this, such as TextRank and SGRank. We can run the algorithm on each character history, sorted by creator, then display the keywords that commonly occur in each character's history for each creator.

8. Question 8
We can run a relatives_alignment_report() to see the top x superheroes with relatives. Start off by pruning irrelevant data, then look for the 10 rows with the most members in the 'relatives' column by converting the dataframe into a json object and iterating through the top 10 results.

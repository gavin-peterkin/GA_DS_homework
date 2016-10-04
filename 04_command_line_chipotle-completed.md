## Lesson 4 Homework: Command Line Chipotle

#### Submitting Your Homework

* Create a Markdown file that includes your answers **and** the code you used to arrive at those answers.
* Add this Markdown file to a GitHub repo that you'll use for all of your coursework.
* Submit a link to your repo using the homework submission form listed just below the course schedule on the class repo on the main README.md page. [(submission form)](https://docs.google.com/forms/d/e/1FAIpQLScH_m8Le4w0sqsvm5uNOTd08p4KDTW8WgnWVP1kFf4CCBi2Ow/viewform)

#### Command Line Tasks

  1. Look at the head and the tail of **chipotle.tsv** in the **data** subdirectory of this repo. Think for a minute about how the data is structured. What do you think each column means? What do you think each row means? Tell me! (If you're unsure, look at more of the file contents.)

  In order to find this information I executed the following commands from the data directory:
  ```
  $ head chipotle.tsv

  $ tail chipotle.tsv
  ```
  The data is tab delimited and has a header row with the following column names:
    * order_id: A unique id for each order (purchase) which may contain multiple items
    * quantity: the number of items
    * item_name: What was ordered from the menu
    * choice_description: For items that have customization options, this column describes those custom options
    * item_price: The price of this item

  2. How many orders do there appear to be?  
  (Using same code as above.) The number of orders is *NOT* equal to the number of lines since several items (lines) can belong to the same order. The total number of orders is 1,834.

  3. How many lines are in this file?
  ```
  $ wc -l chipotle.tsv
  ```
  Answer: 4,623 lines

  4. Which burrito is more popular, steak or chicken?
  ```
  $ csvstat chipotle.tsv
  ```
  Reveals that the *chicken* burrito is more popular. 553 vs. 336 steak.

  5. Do chicken burritos more often have black beans or pinto beans?
  Unfortunately, I know have to convert the .tsv format into the easier to work with .csv and for some reason the csvkit utility doesn't have a way of coping with tab-separated files. (Lame)
  ```
  $ touch chipotle.csv
  $ cat chipotle.tsv | tr "\\t" "," >> chipotle.csv
  $ csvcut -c item_name,choice_description,item_price chipotle.csv | csvgrep -c item_name -m "Chicken Burrito" | csvstat
  ```
  Chicken burritos more often have black beans (62 to 37). It is worth noting also that my csv file is wrong because apparently there were tabs in the choice description field (I don't know why chipotle would do that).

  6. Make a list of all of the CSV or TSV files in the [our class repo] (https://github.com/ga-students/DS-SEA-3). repo (using a single command). You will be working on your local repo on your laptop.  Think about how wildcard characters can help you with this task.
  ```
  # From the root of the class repo

  $ find . -regex ".*\.[tc]sv"
  ./data/Airline_on_time_west_coast.csv
  ./data/airlines.csv
  ./data/bank-additional.csv
  ./data/bikeshare.csv
  ./data/chipotle.csv  # note that I made this one
  ./data/chipotle.tsv
  ./data/citibike_feb2014.csv
  ./data/drinks.csv
  ./data/drones.csv
  ./data/hitters.csv
  ./data/icecream.csv
  ./data/imdb_1000.csv
  ./data/mtcars.csv
  ./data/NBA_players_2015.csv
  ./data/ozone.csv
  ./data/pronto_cycle_share/2015_station_data.csv
  ./data/pronto_cycle_share/2015_trip_data.csv
  ./data/pronto_cycle_share/2015_weather_data.csv
  ./data/rossmann.csv
  ./data/rt_critics.csv
  ./data/sms.tsv
  ./data/stores.csv
  ./data/syria.csv
  ./data/time_series_train.csv
  ./data/titanic.csv
  ./data/ufo.csv
  ./data/vehicles_test.csv
  ./data/vehicles_train.csv
  ./data/yelp.csv
  ```
  7. Count the approximate number of occurrences of the word "dictionary" (regardless of case) across all files of [our class repo] (https://github.com/ga-students/DS-SEA-3).
  ```
  $ grep -irohe "dictionary" . | wc -w
  ```
  There are 86 occurences of the word *dictionary*.
8. **Optional:** Use the the command line to discover something "interesting" about the Chipotle data. Try using the commands from the "advanced" section!

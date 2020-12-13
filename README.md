## 1. TV, halftime shows, and the Big Game
<p>Whether or not you like football, the Super Bowl is a spectacle. There's a little something for everyone at your Super Bowl party. Drama in the form of blowouts, comebacks, and controversy for the sports fan. There are the ridiculously expensive ads, some hilarious, others gut-wrenching, thought-provoking, and weird. The half-time shows with the biggest musicians in the world, sometimes <a href="https://youtu.be/ZD1QrIe--_Y?t=14">riding giant mechanical tigers</a> or <a href="https://youtu.be/mjrdywp5nyE?t=62">leaping from the roof of the stadium</a>. It's a show, baby. And in this notebook, we're going to find out how some of the elements of this show interact with each other. After exploring and cleaning our data a little, we're going to answer questions like:</p>
<ul>
<li>What are the most extreme game outcomes?</li>
<li>How does the game affect television viewership?</li>
<li>How have viewership, TV ratings, and ad cost evolved over time?</li>
<li>Who are the most prolific musicians in terms of halftime show performances?</li>
</ul>
<p><img src="https://assets.datacamp.com/production/project_684/img/left_shark.jpg" alt="Left Shark Steals The Show">
<em><a href="https://www.flickr.com/photos/huntleypaton/16464994135/in/photostream/">Left Shark Steals The Show</a>. Katy Perry performing at halftime of Super Bowl XLIX. Photo by Huntley Paton. Attribution-ShareAlike 2.0 Generic (CC BY-SA 2.0).</em></p>
<p>The dataset we'll use was <a href="https://en.wikipedia.org/wiki/Web_scraping">scraped</a> and polished from Wikipedia. It is made up of three CSV files, one with <a href="https://en.wikipedia.org/wiki/List_of_Super_Bowl_champions">game data</a>, one with <a href="https://en.wikipedia.org/wiki/Super_Bowl_television_ratings">TV data</a>, and one with <a href="https://en.wikipedia.org/wiki/List_of_Super_Bowl_halftime_shows">halftime musician data</a> for all 52 Super Bowls through 2018. Let's take a look, using <code>display()</code> instead of <code>print()</code> since its output is much prettier in Jupyter Notebooks.</p>


```python
# Import pandas
import pandas as pd

# Load the CSV data into DataFrames
super_bowls = pd.read_csv('datasets/super_bowls.csv')
tv = pd.read_csv('datasets/tv.csv')
halftime_musicians = pd.read_csv('datasets/halftime_musicians.csv')

# Display the first five rows of each DataFrame
display(super_bowls.head(5))
display(tv.head(5))
display(halftime_musicians.head(5))
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>super_bowl</th>
      <th>venue</th>
      <th>city</th>
      <th>state</th>
      <th>attendance</th>
      <th>team_winner</th>
      <th>winning_pts</th>
      <th>qb_winner_1</th>
      <th>qb_winner_2</th>
      <th>coach_winner</th>
      <th>team_loser</th>
      <th>losing_pts</th>
      <th>qb_loser_1</th>
      <th>qb_loser_2</th>
      <th>coach_loser</th>
      <th>combined_pts</th>
      <th>difference_pts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-02-04</td>
      <td>52</td>
      <td>U.S. Bank Stadium</td>
      <td>Minneapolis</td>
      <td>Minnesota</td>
      <td>67612</td>
      <td>Philadelphia Eagles</td>
      <td>41</td>
      <td>Nick Foles</td>
      <td>NaN</td>
      <td>Doug Pederson</td>
      <td>New England Patriots</td>
      <td>33</td>
      <td>Tom Brady</td>
      <td>NaN</td>
      <td>Bill Belichick</td>
      <td>74</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017-02-05</td>
      <td>51</td>
      <td>NRG Stadium</td>
      <td>Houston</td>
      <td>Texas</td>
      <td>70807</td>
      <td>New England Patriots</td>
      <td>34</td>
      <td>Tom Brady</td>
      <td>NaN</td>
      <td>Bill Belichick</td>
      <td>Atlanta Falcons</td>
      <td>28</td>
      <td>Matt Ryan</td>
      <td>NaN</td>
      <td>Dan Quinn</td>
      <td>62</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-02-07</td>
      <td>50</td>
      <td>Levi's Stadium</td>
      <td>Santa Clara</td>
      <td>California</td>
      <td>71088</td>
      <td>Denver Broncos</td>
      <td>24</td>
      <td>Peyton Manning</td>
      <td>NaN</td>
      <td>Gary Kubiak</td>
      <td>Carolina Panthers</td>
      <td>10</td>
      <td>Cam Newton</td>
      <td>NaN</td>
      <td>Ron Rivera</td>
      <td>34</td>
      <td>14</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2015-02-01</td>
      <td>49</td>
      <td>University of Phoenix Stadium</td>
      <td>Glendale</td>
      <td>Arizona</td>
      <td>70288</td>
      <td>New England Patriots</td>
      <td>28</td>
      <td>Tom Brady</td>
      <td>NaN</td>
      <td>Bill Belichick</td>
      <td>Seattle Seahawks</td>
      <td>24</td>
      <td>Russell Wilson</td>
      <td>NaN</td>
      <td>Pete Carroll</td>
      <td>52</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2014-02-02</td>
      <td>48</td>
      <td>MetLife Stadium</td>
      <td>East Rutherford</td>
      <td>New Jersey</td>
      <td>82529</td>
      <td>Seattle Seahawks</td>
      <td>43</td>
      <td>Russell Wilson</td>
      <td>NaN</td>
      <td>Pete Carroll</td>
      <td>Denver Broncos</td>
      <td>8</td>
      <td>Peyton Manning</td>
      <td>NaN</td>
      <td>John Fox</td>
      <td>51</td>
      <td>35</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>super_bowl</th>
      <th>network</th>
      <th>avg_us_viewers</th>
      <th>total_us_viewers</th>
      <th>rating_household</th>
      <th>share_household</th>
      <th>rating_18_49</th>
      <th>share_18_49</th>
      <th>ad_cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>52</td>
      <td>NBC</td>
      <td>103390000</td>
      <td>NaN</td>
      <td>43.1</td>
      <td>68</td>
      <td>33.4</td>
      <td>78.0</td>
      <td>5000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>51</td>
      <td>Fox</td>
      <td>111319000</td>
      <td>172000000.0</td>
      <td>45.3</td>
      <td>73</td>
      <td>37.1</td>
      <td>79.0</td>
      <td>5000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>50</td>
      <td>CBS</td>
      <td>111864000</td>
      <td>167000000.0</td>
      <td>46.6</td>
      <td>72</td>
      <td>37.7</td>
      <td>79.0</td>
      <td>5000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>49</td>
      <td>NBC</td>
      <td>114442000</td>
      <td>168000000.0</td>
      <td>47.5</td>
      <td>71</td>
      <td>39.1</td>
      <td>79.0</td>
      <td>4500000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>48</td>
      <td>Fox</td>
      <td>112191000</td>
      <td>167000000.0</td>
      <td>46.7</td>
      <td>69</td>
      <td>39.3</td>
      <td>77.0</td>
      <td>4000000</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>super_bowl</th>
      <th>musician</th>
      <th>num_songs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>52</td>
      <td>Justin Timberlake</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>52</td>
      <td>University of Minnesota Marching Band</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>51</td>
      <td>Lady Gaga</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50</td>
      <td>Coldplay</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50</td>
      <td>Beyoncé</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

def test_pandas_loaded():
    assert 'pd' in globals(), \
    'Did you import the pandas module under the alias pd?'
    
def test_super_bowls_correctly_loaded():
    correct_super_bowls = pd.read_csv('datasets/super_bowls.csv')
    assert correct_super_bowls.equals(super_bowls), "The variable super_bowls does not contain the data in super_bowls.csv."

def test_tv_correctly_loaded():
    correct_tv = pd.read_csv('datasets/tv.csv')
    assert correct_tv.equals(tv), "The variable tv does not contain the data in tv.csv."
    
def test_halftime_musicians_correctly_loaded():
    correct_halftime_musicians = pd.read_csv('datasets/halftime_musicians.csv')
    assert correct_halftime_musicians.equals(halftime_musicians), "The variable halftime_musicians does not contain the data in halftime_musicians.csv."
```






    4/4 tests passed




## 2. Taking note of dataset issues
<p>For the Super Bowl game data, we can see the dataset appears whole except for missing values in the backup quarterback columns (<code>qb_winner_2</code> and <code>qb_loser_2</code>), which make sense given most starting QBs in the Super Bowl (<code>qb_winner_1</code> and <code>qb_loser_1</code>) play the entire game.</p>
<p>From the visual inspection of TV and halftime musicians data, there is only one missing value displayed, but I've got a hunch there are more. The Super Bowl goes all the way back to 1967, and the more granular columns (e.g. the number of songs for halftime musicians) probably weren't tracked reliably over time. Wikipedia is great but not perfect.</p>
<p>An inspection of the <code>.info()</code> output for <code>tv</code> and <code>halftime_musicians</code> shows us that there are multiple columns with null values.</p>


```python
# Summary of the TV data to inspect
tv.info()

print('\n')

# Summary of the halftime musician data to inspect
halftime_musicians.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 53 entries, 0 to 52
    Data columns (total 9 columns):
    super_bowl          53 non-null int64
    network             53 non-null object
    avg_us_viewers      53 non-null int64
    total_us_viewers    15 non-null float64
    rating_household    53 non-null float64
    share_household     53 non-null int64
    rating_18_49        15 non-null float64
    share_18_49         6 non-null float64
    ad_cost             53 non-null int64
    dtypes: float64(4), int64(4), object(1)
    memory usage: 3.8+ KB
    
    
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 134 entries, 0 to 133
    Data columns (total 3 columns):
    super_bowl    134 non-null int64
    musician      134 non-null object
    num_songs     88 non-null float64
    dtypes: float64(1), int64(1), object(1)
    memory usage: 3.2+ KB



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

# def test_nothing_task_2():
#     assert True, "Nothing to test."

last_input = In[-2]

def test_not_empty_task_2():
    assert "# ... YOUR CODE FOR TASK" not in last_input, \
        "It appears that # ... YOUR CODE FOR TASK X ... is still in the code cell, which suggests that you might not have attempted the code for this task. If you have, please delete # ... YOUR CODE FOR TASK X ... from the cell and resubmit."
```






    1/1 tests passed




## 3. Combined points distribution
<p>For the TV data, the following columns have missing values and a lot of them:</p>
<ul>
<li><code>total_us_viewers</code> (amount of U.S. viewers who watched at least some part of the broadcast)</li>
<li><code>rating_18_49</code> (average % of U.S. adults 18-49 who live in a household with a TV that were watching for the entire broadcast)</li>
<li><code>share_18_49</code> (average % of U.S. adults 18-49 who live in a household with a TV <em>in use</em> that were watching for the entire broadcast)</li>
</ul>
<p>For the halftime musician data, there are missing numbers of songs performed (<code>num_songs</code>) for about a third of the performances.</p>
<p>There are a lot of potential reasons for these missing values. Was the data ever tracked? Was it lost in history? Is the research effort to make this data whole worth it? Maybe. Watching every Super Bowl halftime show to get song counts would be pretty fun. But we don't have the time to do that kind of stuff now! Let's take note of where the dataset isn't perfect and start uncovering some insights.</p>
<p>Let's start by looking at combined points for each Super Bowl by visualizing the distribution. Let's also pinpoint the Super Bowls with the highest and lowest scores.</p>


```python
# Import matplotlib and set plotting style
from matplotlib import pyplot as plt
%matplotlib inline
plt.style.use('seaborn')

# Plot a histogram of combined points
plt.hist(super_bowls.combined_pts)
plt.xlabel('Combined Points')
plt.ylabel('Number of Super Bowls')
plt.show()

# Display the Super Bowls with the highest and lowest combined scores
display(super_bowls[super_bowls['combined_pts'] > 70])
display(super_bowls[super_bowls['combined_pts'] < 25])
```

    /usr/local/lib/python3.6/dist-packages/matplotlib/figure.py:2299: UserWarning: This figure includes Axes that are not compatible with tight_layout, so results might be incorrect.
      warnings.warn("This figure includes Axes that are not compatible "



![png](output_7_1.png)



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>super_bowl</th>
      <th>venue</th>
      <th>city</th>
      <th>state</th>
      <th>attendance</th>
      <th>team_winner</th>
      <th>winning_pts</th>
      <th>qb_winner_1</th>
      <th>qb_winner_2</th>
      <th>coach_winner</th>
      <th>team_loser</th>
      <th>losing_pts</th>
      <th>qb_loser_1</th>
      <th>qb_loser_2</th>
      <th>coach_loser</th>
      <th>combined_pts</th>
      <th>difference_pts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-02-04</td>
      <td>52</td>
      <td>U.S. Bank Stadium</td>
      <td>Minneapolis</td>
      <td>Minnesota</td>
      <td>67612</td>
      <td>Philadelphia Eagles</td>
      <td>41</td>
      <td>Nick Foles</td>
      <td>NaN</td>
      <td>Doug Pederson</td>
      <td>New England Patriots</td>
      <td>33</td>
      <td>Tom Brady</td>
      <td>NaN</td>
      <td>Bill Belichick</td>
      <td>74</td>
      <td>8</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1995-01-29</td>
      <td>29</td>
      <td>Joe Robbie Stadium</td>
      <td>Miami Gardens</td>
      <td>Florida</td>
      <td>74107</td>
      <td>San Francisco 49ers</td>
      <td>49</td>
      <td>Steve Young</td>
      <td>NaN</td>
      <td>George Seifert</td>
      <td>San Diego Chargers</td>
      <td>26</td>
      <td>Stan Humphreys</td>
      <td>NaN</td>
      <td>Bobby Ross</td>
      <td>75</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>super_bowl</th>
      <th>venue</th>
      <th>city</th>
      <th>state</th>
      <th>attendance</th>
      <th>team_winner</th>
      <th>winning_pts</th>
      <th>qb_winner_1</th>
      <th>qb_winner_2</th>
      <th>coach_winner</th>
      <th>team_loser</th>
      <th>losing_pts</th>
      <th>qb_loser_1</th>
      <th>qb_loser_2</th>
      <th>coach_loser</th>
      <th>combined_pts</th>
      <th>difference_pts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>43</th>
      <td>1975-01-12</td>
      <td>9</td>
      <td>Tulane Stadium</td>
      <td>New Orleans</td>
      <td>Louisiana</td>
      <td>80997</td>
      <td>Pittsburgh Steelers</td>
      <td>16</td>
      <td>Terry Bradshaw</td>
      <td>NaN</td>
      <td>Chuck Noll</td>
      <td>Minnesota Vikings</td>
      <td>6</td>
      <td>Fran Tarkenton</td>
      <td>NaN</td>
      <td>Bud Grant</td>
      <td>22</td>
      <td>10</td>
    </tr>
    <tr>
      <th>45</th>
      <td>1973-01-14</td>
      <td>7</td>
      <td>Memorial Coliseum</td>
      <td>Los Angeles</td>
      <td>California</td>
      <td>90182</td>
      <td>Miami Dolphins</td>
      <td>14</td>
      <td>Bob Griese</td>
      <td>NaN</td>
      <td>Don Shula</td>
      <td>Washington Redskins</td>
      <td>7</td>
      <td>Bill Kilmer</td>
      <td>NaN</td>
      <td>George Allen</td>
      <td>21</td>
      <td>7</td>
    </tr>
    <tr>
      <th>49</th>
      <td>1969-01-12</td>
      <td>3</td>
      <td>Orange Bowl</td>
      <td>Miami</td>
      <td>Florida</td>
      <td>75389</td>
      <td>New York Jets</td>
      <td>16</td>
      <td>Joe Namath</td>
      <td>NaN</td>
      <td>Weeb Ewbank</td>
      <td>Baltimore Colts</td>
      <td>7</td>
      <td>Earl Morrall</td>
      <td>Johnny Unitas</td>
      <td>Don Shula</td>
      <td>23</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

def test_matplotlib_loaded():
    assert 'plt' in globals(), \
    'Did you import the pyplot module from matplotlib under the alias plt?'
```






    1/1 tests passed




## 4. Point difference distribution
<p>Most combined scores are around 40-50 points, with the extremes being roughly equal distance away in opposite directions. Going up to the highest combined scores at 74 and 75, we find two games featuring dominant quarterback performances. One even happened recently in 2018's Super Bowl LII where Tom Brady's Patriots lost to Nick Foles' underdog Eagles 41-33 for a combined score of 74.</p>
<p>Going down to the lowest combined scores, we have Super Bowl III and VII, which featured tough defenses that dominated. We also have Super Bowl IX in New Orleans in 1975, whose 16-6 score can be attributed to inclement weather. The field was slick from overnight rain, and it was cold at 46 °F (8 °C), making it hard for the Steelers and Vikings to do much offensively. This was the second-coldest Super Bowl ever and the last to be played in inclement weather for over 30 years. The NFL realized people like points, I guess.</p>
<p><em>UPDATE: In Super Bowl LIII in 2019, the Patriots and Rams broke the record for the lowest-scoring Super Bowl with a combined score of 16 points (13-3 for the Patriots).</em></p>
<p>Let's take a look at point <em>difference</em> now.</p>


```python
plt.hist(super_bowls.difference_pts)
plt.xlabel('Point Difference')
plt.ylabel('Number of Super Bowls')
plt.show()

# Display the closest game(s) and biggest blowouts
display(super_bowls[super_bowls['difference_pts'] == 1])
display(super_bowls[super_bowls['difference_pts'] >= 35])
```

    /usr/local/lib/python3.6/dist-packages/matplotlib/figure.py:2299: UserWarning: This figure includes Axes that are not compatible with tight_layout, so results might be incorrect.
      warnings.warn("This figure includes Axes that are not compatible "



![png](output_10_1.png)



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>super_bowl</th>
      <th>venue</th>
      <th>city</th>
      <th>state</th>
      <th>attendance</th>
      <th>team_winner</th>
      <th>winning_pts</th>
      <th>qb_winner_1</th>
      <th>qb_winner_2</th>
      <th>coach_winner</th>
      <th>team_loser</th>
      <th>losing_pts</th>
      <th>qb_loser_1</th>
      <th>qb_loser_2</th>
      <th>coach_loser</th>
      <th>combined_pts</th>
      <th>difference_pts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>27</th>
      <td>1991-01-27</td>
      <td>25</td>
      <td>Tampa Stadium</td>
      <td>Tampa</td>
      <td>Florida</td>
      <td>73813</td>
      <td>New York Giants</td>
      <td>20</td>
      <td>Jeff Hostetler</td>
      <td>NaN</td>
      <td>Bill Parcells</td>
      <td>Buffalo Bills</td>
      <td>19</td>
      <td>Jim Kelly</td>
      <td>NaN</td>
      <td>Marv Levy</td>
      <td>39</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>super_bowl</th>
      <th>venue</th>
      <th>city</th>
      <th>state</th>
      <th>attendance</th>
      <th>team_winner</th>
      <th>winning_pts</th>
      <th>qb_winner_1</th>
      <th>qb_winner_2</th>
      <th>coach_winner</th>
      <th>team_loser</th>
      <th>losing_pts</th>
      <th>qb_loser_1</th>
      <th>qb_loser_2</th>
      <th>coach_loser</th>
      <th>combined_pts</th>
      <th>difference_pts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>2014-02-02</td>
      <td>48</td>
      <td>MetLife Stadium</td>
      <td>East Rutherford</td>
      <td>New Jersey</td>
      <td>82529</td>
      <td>Seattle Seahawks</td>
      <td>43</td>
      <td>Russell Wilson</td>
      <td>NaN</td>
      <td>Pete Carroll</td>
      <td>Denver Broncos</td>
      <td>8</td>
      <td>Peyton Manning</td>
      <td>NaN</td>
      <td>John Fox</td>
      <td>51</td>
      <td>35</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1993-01-31</td>
      <td>27</td>
      <td>Rose Bowl</td>
      <td>Pasadena</td>
      <td>California</td>
      <td>98374</td>
      <td>Dallas Cowboys</td>
      <td>52</td>
      <td>Troy Aikman</td>
      <td>NaN</td>
      <td>Jimmy Johnson</td>
      <td>Buffalo Bills</td>
      <td>17</td>
      <td>Jim Kelly</td>
      <td>Frank Reich</td>
      <td>Marv Levy</td>
      <td>69</td>
      <td>35</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1990-01-28</td>
      <td>24</td>
      <td>Louisiana Superdome</td>
      <td>New Orleans</td>
      <td>Louisiana</td>
      <td>72919</td>
      <td>San Francisco 49ers</td>
      <td>55</td>
      <td>Joe Montana</td>
      <td>NaN</td>
      <td>George Seifert</td>
      <td>Denver Broncos</td>
      <td>10</td>
      <td>John Elway</td>
      <td>NaN</td>
      <td>Dan Reeves</td>
      <td>65</td>
      <td>45</td>
    </tr>
    <tr>
      <th>32</th>
      <td>1986-01-26</td>
      <td>20</td>
      <td>Louisiana Superdome</td>
      <td>New Orleans</td>
      <td>Louisiana</td>
      <td>73818</td>
      <td>Chicago Bears</td>
      <td>46</td>
      <td>Jim McMahon</td>
      <td>NaN</td>
      <td>Mike Ditka</td>
      <td>New England Patriots</td>
      <td>10</td>
      <td>Tony Eason</td>
      <td>Steve Grogan</td>
      <td>Raymond Berry</td>
      <td>56</td>
      <td>36</td>
    </tr>
  </tbody>
</table>
</div>



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

last_input = In[-2]

def test_not_empty_task_4():
    assert "# ... YOUR CODE FOR TASK" not in last_input, \
        "It appears that # ... YOUR CODE FOR TASK X ... is still in the code cell, which suggests that you might not have attempted the code for this task. If you have, please delete # ... YOUR CODE FOR TASK X ... from the cell and resubmit."
```






    1/1 tests passed




## 5. Do blowouts translate to lost viewers?
<p>The vast majority of Super Bowls are close games. Makes sense. Both teams are likely to be deserving if they've made it this far. The closest game ever was when the Buffalo Bills lost to the New York Giants by 1 point in 1991, which was  best remembered for Scott Norwood's last-second missed field goal attempt that went <em><a href="https://www.youtube.com/watch?v=RPFZCGgjDSg">wide right</a></em>, kicking off four Bills Super Bowl losses in a row. Poor Scott. The biggest point discrepancy ever was 45 points (!) where Hall of Famer Joe Montana's led the San Francisco 49ers to victory in 1990, one year before the closest game ever.</p>
<p>I remember watching the Seahawks crush the Broncos by 35 points (43-8) in 2014, which was a boring experience in my opinion. The game was never really close. I'm pretty sure we changed the channel at the end of the third quarter. Let's combine our game data and TV to see if this is a universal phenomenon. Do large point differences translate to lost viewers? We can plot <a href="https://en.wikipedia.org/wiki/Nielsen_ratings">household share</a> <em>(average percentage of U.S. households with a TV in use that were watching for the entire broadcast)</em> vs. point difference to find out.</p>


```python
# Join game and TV data, filtering out SB I because it was split over two networks
games_tv = pd.merge(tv[tv['super_bowl'] > 1], super_bowls, on='super_bowl')

# Import seaborn
import seaborn as sns 

# Create a scatter plot with a linear regression model fit
sns.regplot(x='difference_pts', y='share_household', data=games_tv)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f20936fdc50>



    /usr/local/lib/python3.6/dist-packages/matplotlib/figure.py:2299: UserWarning: This figure includes Axes that are not compatible with tight_layout, so results might be incorrect.
      warnings.warn("This figure includes Axes that are not compatible "



![png](output_13_2.png)



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

last_value = _

def test_seaborn_loaded():
    assert 'sns' in globals(), \
    'Did you import the seaborn module under the alias sns?'

def test_plot_exists_5():
    try:
        assert type(last_value) == type(sns.regplot(x='difference_pts', y='share_household', data=games_tv))
    except AssertionError:
        assert False, 'A plot was not the last output of the code cell.'
```






    2/2 tests passed




    /usr/local/lib/python3.6/dist-packages/matplotlib/figure.py:2299: UserWarning: This figure includes Axes that are not compatible with tight_layout, so results might be incorrect.
      warnings.warn("This figure includes Axes that are not compatible "



![png](output_14_3.png)


## 6. Viewership and the ad industry over time
<p>The downward sloping regression line and the 95% confidence interval for that regression <em>suggest</em> that bailing on the game if it is a blowout is common. Though it matches our intuition, we must take it with a grain of salt because the linear relationship in the data is weak due to our small sample size of 52 games.</p>
<p>Regardless of the score though, I bet most people stick it out for the halftime show, which is good news for the TV networks and advertisers. A 30-second spot costs a pretty <a href="https://www.businessinsider.com/super-bowl-commercials-cost-more-than-eagles-quarterback-earns-2018-1">\$5 million</a> now, but has it always been that way? And how have number of viewers and household ratings trended alongside ad cost? We can find out using line plots that share a "Super Bowl" x-axis.</p>


```python
# Create a figure with 3x1 subplot and activate the top subplot
plt.subplot(3, 1, 1)
plt.plot(tv.super_bowl, tv.avg_us_viewers, color='#648FFF')
plt.title('Average Number of US Viewers')

# Activate the middle subplot
plt.subplot(3, 1, 2)
plt.plot(tv.super_bowl, tv.rating_household, color='#DC267F')
plt.title('Household Rating')

# Activate the bottom subplot
plt.subplot(3, 1, 3)
plt.plot(tv.super_bowl, tv.ad_cost, color='#FFB000')
plt.title('Ad Cost')
plt.xlabel('SUPER BOWL')

# Improve the spacing between subplots
plt.tight_layout()
```

    /usr/local/lib/python3.6/dist-packages/matplotlib/figure.py:2299: UserWarning: This figure includes Axes that are not compatible with tight_layout, so results might be incorrect.
      warnings.warn("This figure includes Axes that are not compatible "



![png](output_16_1.png)



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

last_input = In[-2]

def test_not_empty_task_6():
    assert "# ... YOUR CODE FOR TASK" not in last_input, \
        "It appears that # ... YOUR CODE FOR TASK X ... is still in the code cell, which suggests that you might not have attempted the code for this task. If you have, please delete # ... YOUR CODE FOR TASK X ... from the cell and resubmit."
```






    1/1 tests passed




## 7. Halftime shows weren't always this great
<p>We can see viewers increased before ad costs did. Maybe the networks weren't very data savvy and were slow to react? Makes sense since DataCamp didn't exist back then.</p>
<p>Another hypothesis: maybe halftime shows weren't that good in the earlier years? The modern spectacle of the Super Bowl has a lot to do with the cultural prestige of big halftime acts. I went down a YouTube rabbit hole and it turns out the old ones weren't up to today's standards. Some offenders:</p>
<ul>
<li><a href="https://youtu.be/6wMXHxWO4ns?t=263">Super Bowl XXVI</a> in 1992: A Frosty The Snowman rap performed by children.</li>
<li><a href="https://www.youtube.com/watch?v=PKQTL1PYSag">Super Bowl XXIII</a> in 1989: An Elvis impersonator that did magic tricks and didn't even sing one Elvis song.</li>
<li><a href="https://youtu.be/oSXMNbK2e98?t=436">Super Bowl XXI</a> in 1987: Tap dancing ponies. (Okay, that's pretty awesome actually.)</li>
</ul>
<p>It turns out Michael Jackson's Super Bowl XXVII performance, one of the most watched events in American TV history, was when the NFL realized the value of Super Bowl airtime and decided they needed to sign big name acts from then on out. The halftime shows before MJ indeed weren't that impressive, which we can see by filtering our <code>halftime_musician</code> data.</p>


```python
# Display all halftime musicians for Super Bowls up to and including Super Bowl XXVII
halftime_musicians[halftime_musicians.super_bowl<27]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>super_bowl</th>
      <th>musician</th>
      <th>num_songs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>81</th>
      <td>26</td>
      <td>Gloria Estefan</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>82</th>
      <td>26</td>
      <td>University of Minnesota Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>83</th>
      <td>25</td>
      <td>New Kids on the Block</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>84</th>
      <td>24</td>
      <td>Pete Fountain</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>85</th>
      <td>24</td>
      <td>Doug Kershaw</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>86</th>
      <td>24</td>
      <td>Irma Thomas</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>87</th>
      <td>24</td>
      <td>Pride of Nicholls Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>88</th>
      <td>24</td>
      <td>The Human Jukebox</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>89</th>
      <td>24</td>
      <td>Pride of Acadiana</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>90</th>
      <td>23</td>
      <td>Elvis Presto</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>91</th>
      <td>22</td>
      <td>Chubby Checker</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>92</th>
      <td>22</td>
      <td>San Diego State University Marching Aztecs</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>93</th>
      <td>22</td>
      <td>Spirit of Troy</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>94</th>
      <td>21</td>
      <td>Grambling State University Tiger Marching Band</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>95</th>
      <td>21</td>
      <td>Spirit of Troy</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>96</th>
      <td>20</td>
      <td>Up with People</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>97</th>
      <td>19</td>
      <td>Tops In Blue</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>98</th>
      <td>18</td>
      <td>The University of Florida Fightin' Gator March...</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>99</th>
      <td>18</td>
      <td>The Florida State University Marching Chiefs</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>100</th>
      <td>17</td>
      <td>Los Angeles Unified School District All City H...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>101</th>
      <td>16</td>
      <td>Up with People</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>102</th>
      <td>15</td>
      <td>The Human Jukebox</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>103</th>
      <td>15</td>
      <td>Helen O'Connell</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>104</th>
      <td>14</td>
      <td>Up with People</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>105</th>
      <td>14</td>
      <td>Grambling State University Tiger Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>106</th>
      <td>13</td>
      <td>Ken Hamilton</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>107</th>
      <td>13</td>
      <td>Gramacks</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>108</th>
      <td>12</td>
      <td>Tyler Junior College Apache Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>109</th>
      <td>12</td>
      <td>Pete Fountain</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>110</th>
      <td>12</td>
      <td>Al Hirt</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>111</th>
      <td>11</td>
      <td>Los Angeles Unified School District All City H...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>112</th>
      <td>10</td>
      <td>Up with People</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>113</th>
      <td>9</td>
      <td>Mercer Ellington</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>114</th>
      <td>9</td>
      <td>Grambling State University Tiger Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>115</th>
      <td>8</td>
      <td>University of Texas Longhorn Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>116</th>
      <td>8</td>
      <td>Judy Mallett</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>117</th>
      <td>7</td>
      <td>University of Michigan Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>118</th>
      <td>7</td>
      <td>Woody Herman</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>119</th>
      <td>7</td>
      <td>Andy Williams</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>120</th>
      <td>6</td>
      <td>Ella Fitzgerald</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>121</th>
      <td>6</td>
      <td>Carol Channing</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>122</th>
      <td>6</td>
      <td>Al Hirt</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>123</th>
      <td>6</td>
      <td>United States Air Force Academy Cadet Chorale</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>124</th>
      <td>5</td>
      <td>Southeast Missouri State Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125</th>
      <td>4</td>
      <td>Marguerite Piazza</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>126</th>
      <td>4</td>
      <td>Doc Severinsen</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>127</th>
      <td>4</td>
      <td>Al Hirt</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>128</th>
      <td>4</td>
      <td>The Human Jukebox</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>129</th>
      <td>3</td>
      <td>Florida A&amp;M University Marching 100 Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>130</th>
      <td>2</td>
      <td>Grambling State University Tiger Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>131</th>
      <td>1</td>
      <td>University of Arizona Symphonic Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>132</th>
      <td>1</td>
      <td>Grambling State University Tiger Marching Band</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>133</th>
      <td>1</td>
      <td>Al Hirt</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

last_value = _
        
def test_head_output():
    try:
        assert "Wynonna Judd" not in last_value.to_string()
    except AttributeError:
        assert False, "Please do not use the display() or print() functions to display the filtered DataFrame. Write your line of code as the last line in the cell instead."
    except AssertionError:
        assert False, "Hmm, it seems halftime_musicians wasn't filtered correctly and/or displayed as the last output of the cell. Michael Jackson's performance should be the first row displayed."
```






    1/1 tests passed




## 8. Who has the most halftime show appearances?
<p>Lots of marching bands. American jazz clarinetist Pete Fountain. Miss Texas 1973 playing a violin. Nothing against those performers, they're just simply not <a href="https://www.youtube.com/watch?v=suIg9kTGBVI">Beyoncé</a>. To be fair, no one is.</p>
<p>Let's see all of the musicians that have done more than one halftime show, including their performance counts.</p>


```python
# Count halftime show appearances for each musician and sort them from most to least
halftime_appearances = halftime_musicians.groupby('musician').count()['super_bowl'].reset_index()
halftime_appearances = halftime_appearances.sort_values('super_bowl', ascending=False)

# Display musicians with more than one halftime show appearance
halftime_appearances[halftime_appearances.super_bowl>1]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>musician</th>
      <th>super_bowl</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>28</th>
      <td>Grambling State University Tiger Marching Band</td>
      <td>6</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Up with People</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Al Hirt</td>
      <td>4</td>
    </tr>
    <tr>
      <th>83</th>
      <td>The Human Jukebox</td>
      <td>3</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Spirit of Troy</td>
      <td>2</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Florida A&amp;M University Marching 100 Band</td>
      <td>2</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Gloria Estefan</td>
      <td>2</td>
    </tr>
    <tr>
      <th>102</th>
      <td>University of Minnesota Marching Band</td>
      <td>2</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Bruno Mars</td>
      <td>2</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Pete Fountain</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Beyoncé</td>
      <td>2</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Justin Timberlake</td>
      <td>2</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Nelly</td>
      <td>2</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Los Angeles Unified School District All City H...</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

last_value = _

def test_filter_correct_8():
    try:
        assert len(last_value) == 14
    except TypeError:
        assert False, "Hmm, it seems halftime_appearances wasn't filtered correctly and/or displayed as the last line of code in the cell (i.e., displayed without the display() or print() functions). There should be 14 repeat halftime show acts."
    except AssertionError:
        assert False, "Hmm, it seems halftime_appearances wasn't filtered correctly. There should be 14 repeat halftime show acts."
```






    1/1 tests passed




## 9. Who performed the most songs in a halftime show?
<p>The world famous <a href="https://www.youtube.com/watch?v=RL_3oqpHiDg">Grambling State University Tiger Marching Band</a> takes the crown with six appearances. Beyoncé, Justin Timberlake, Nelly, and Bruno Mars are the only post-Y2K musicians with multiple appearances (two each).</p>
<p>From our previous inspections, the <code>num_songs</code> column has lots of missing values:</p>
<ul>
<li>A lot of the marching bands don't have <code>num_songs</code> entries.</li>
<li>For non-marching bands, missing data starts occurring at Super Bowl XX.</li>
</ul>
<p>Let's filter out marching bands by filtering out musicians with the word "Marching" in them and the word "Spirit" (a common naming convention for marching bands is "Spirit of [something]"). Then we'll filter for Super Bowls after Super Bowl XX to address the missing data issue, <em>then</em> let's see who has the most number of songs.</p>


```python
# Filter out most marching bands
no_bands = halftime_musicians[~halftime_musicians.musician.str.contains('Marching')]
no_bands = no_bands[~no_bands.musician.str.contains('Spirit')]

# Plot a histogram of number of songs per performance
most_songs = int(max(no_bands['num_songs'].values))
plt.hist(most_songs)
plt.hist(super_bowls.difference_pts)
plt.ylabel('Number of Musicians')
plt.show()

# Sort the non-band musicians by number of songs per appearance...
no_bands = no_bands.sort_values('num_songs', ascending=False)
plt.xlabel('Number of Songs Per Halftime Show Performance')
display(no_bands.head(15))
```

    /usr/local/lib/python3.6/dist-packages/matplotlib/figure.py:2299: UserWarning: This figure includes Axes that are not compatible with tight_layout, so results might be incorrect.
      warnings.warn("This figure includes Axes that are not compatible "



![png](output_25_1.png)



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>super_bowl</th>
      <th>musician</th>
      <th>num_songs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>52</td>
      <td>Justin Timberlake</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>70</th>
      <td>30</td>
      <td>Diana Ross</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>49</td>
      <td>Katy Perry</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>51</td>
      <td>Lady Gaga</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>90</th>
      <td>23</td>
      <td>Elvis Presto</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>41</td>
      <td>Prince</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>47</td>
      <td>Beyoncé</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>48</td>
      <td>Bruno Mars</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50</td>
      <td>Coldplay</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>45</td>
      <td>The Black Eyed Peas</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>46</td>
      <td>Madonna</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>44</td>
      <td>The Who</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>80</th>
      <td>27</td>
      <td>Michael Jackson</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>64</th>
      <td>32</td>
      <td>The Temptations</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>36</th>
      <td>39</td>
      <td>Paul McCartney</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>


    /usr/local/lib/python3.6/dist-packages/matplotlib/figure.py:2299: UserWarning: This figure includes Axes that are not compatible with tight_layout, so results might be incorrect.
      warnings.warn("This figure includes Axes that are not compatible "



![png](output_25_4.png)



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

last_input = In[-2]

def test_not_empty_task_9():
    assert "# ... YOUR CODE FOR TASK" not in last_input, \
        "It appears that # ... YOUR CODE FOR TASK X ... is still in the code cell, which suggests that you might not have attempted the code for this task. If you have, please delete # ... YOUR CODE FOR TASK X ... from the cell and resubmit."
```






    1/1 tests passed




## 10. Conclusion
<p>So most non-band musicians do 1-3 songs per halftime show. It's important to note that the duration of the halftime show is fixed (roughly 12 minutes) so songs per performance is more a measure of how many hit songs you have. JT went off in 2018, wow. 11 songs! Diana Ross comes in second with 10 in her medley in 1996.</p>
<p>In this notebook, we loaded, cleaned, then explored Super Bowl game, television, and halftime show data. We visualized the distributions of combined points, point differences, and halftime show performances using histograms. We used line plots to see how ad cost increases lagged behind viewership increases. And we discovered that blowouts do appear to lead to a drop in viewers.</p>
<p>This year's Big Game will be here before you know it. Who do you think will win Super Bowl LIII?</p>
<p><em>UPDATE: <a href="https://en.wikipedia.org/wiki/Super_Bowl_LIII">Spoiler alert</a>.</em></p>


```python
# 2018-2019 conference champions
patriots = 'New England Patriots'
rams = 'Los Angeles Rams'

# Who will win Super Bowl LIII?
super_bowl_LIII_winner = patriots
print('The winner of Super Bowl LIII will be the', super_bowl_LIII_winner)
```

    The winner of Super Bowl LIII will be the New England Patriots



```python
%%nose
# %%nose needs to be included at the beginning of every @tests cell

# One or more tests of the student's code
# The @solution should pass the tests
# The purpose of the tests is to try to catch common errors and
# to give the student a hint on how to resolve these errors

def test_valid_winner_chosen():
    assert super_bowl_LIII_winner == 'New England Patriots' or super_bowl_LIII_winner == 'Los Angeles Rams', \
    "It appears a valid potential winner was not selected. Please assign the patriots variable or the rams variable to super_bowl_LIII_winner."
```






    1/1 tests passed




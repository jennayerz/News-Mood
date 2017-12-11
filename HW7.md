

```python
# Dependencies
import tweepy
import matplotlib.pyplot as plt
import pandas as pd
import json
import numpy as np
import time
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
```


```python
# Twitter API keys
consumer_key = 'Hi8F0fFbzX3L3DczlZDobQBCm'
consumer_secret = 'kN2ddiPaMFgqBiqDIqUNyblJd91ODHYphGzb1Aii7q9b7tn8Xv'
access_token = '933136362651262976-mr6EH2ZmyMC3Klw5C9Oapkp04CnymaC'
access_token_secret = 'HCScnHUYgeNz3iXhe1DYbDFLc5sZW6L1dKoy20fJt9FEM'

# Twitter credentials
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
# Create columns for dataframe
sentiments = {'Tweet Number': '',
              'News Organization': '',
              'Tweet': '',
              'Compound Score': '',
              'Positive Score': '',
              'Negative Score': '',
              'Neutral Score': ''}
```


```python
# Create list of news accounts
target_terms = ['@BBC', '@CBS', '@CNN', '@FoxNews', '@nytimes']
```


```python
# Variable to hold list of the compound sentiment
compound_list = []
positive_list = []
negative_list = []
neutral_list = []
tweet_number = []
text = []
news_source = []

# Loop through all target users
for target in target_terms:
        
    # Run search around each tweet
    screen_name = target
    public_tweets = api.user_timeline(target,
                                      count=100)
    
    # Index to see how many tweets for each news source
    index = 0
        
    # Loop through all tweets
    for tweet in public_tweets:
        
        news_organization = target
            
        # Run vader analysis on each tweet
        scores = analyzer.polarity_scores(tweet['text'])
        compound = scores['compound']
        pos = scores['pos']
        neu = scores['neu']
        neg = scores['neg']
            
        # Add value to list
        compound_list.append(compound)
        positive_list.append(pos)
        negative_list.append(neg)
        neutral_list.append(neu)
        text.append(tweet['text'])
        tweet_number.append(index)
        news_source.append(news_organization)
        
        index = index + 1
```


```python
# Add values from list to dataframe
sentiments['Tweet Number'] = tweet_number
sentiments['News Organization'] = news_source
sentiments['Tweet'] = text
sentiments['Compound Score'] = compound_list
sentiments['Positive Score'] = positive_list
sentiments['Negative Score'] = negative_list
sentiments['Neutral Score'] = neutral_list

sentiments_df = pd.DataFrame(sentiments)
sentiments_df.to_csv('sentimentsnewsmedia.csv')
sentiments_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Compound Score</th>
      <th>Negative Score</th>
      <th>Neutral Score</th>
      <th>News Organization</th>
      <th>Positive Score</th>
      <th>Tweet</th>
      <th>Tweet Number</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.6915</td>
      <td>0.000</td>
      <td>0.560</td>
      <td>@BBC</td>
      <td>0.440</td>
      <td>RT @BBCOne: SO. MUCH. CUTE. 😍\n#Attenboroughan...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCEarth: 'Never before have we had such a...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.4391</td>
      <td>0.000</td>
      <td>0.855</td>
      <td>@BBC</td>
      <td>0.145</td>
      <td>🌹@DuaLipa performing 'Homesick' was a complete...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.3818</td>
      <td>0.126</td>
      <td>0.874</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCEarth: 'What shocks me ...is how fast t...</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCOne: If we don’t act, coral reefs could...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCEarth: In 1986, many nations decided to...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>Little things can make a big difference.\n#Blu...</td>
      <td>6</td>
    </tr>
    <tr>
      <th>7</th>
      <td>-0.5574</td>
      <td>0.231</td>
      <td>0.769</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCSpringwatch: Manmade materials are dest...</td>
      <td>7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>-0.8360</td>
      <td>0.283</td>
      <td>0.717</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCEarth: It’s estimated that tens of mill...</td>
      <td>8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCOne: These are *all* items regurgitated...</td>
      <td>9</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.4019</td>
      <td>0.000</td>
      <td>0.870</td>
      <td>@BBC</td>
      <td>0.130</td>
      <td>RT @BBCOne: Our rubbish, our responsibility. ✊...</td>
      <td>10</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.9169</td>
      <td>0.000</td>
      <td>0.482</td>
      <td>@BBC</td>
      <td>0.518</td>
      <td>Eight delightful dolphin photos that'll inspir...</td>
      <td>11</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCEarth: ‘We’re only now beginning to rea...</td>
      <td>12</td>
    </tr>
    <tr>
      <th>13</th>
      <td>-0.5267</td>
      <td>0.152</td>
      <td>0.848</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCOne: "The oceans are under threat now a...</td>
      <td>13</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCEarth: The countdown is on! The final e...</td>
      <td>14</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.5106</td>
      <td>0.000</td>
      <td>0.837</td>
      <td>@BBC</td>
      <td>0.163</td>
      <td>RT @BBCSpringwatch: Seven British conservation...</td>
      <td>15</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0.7096</td>
      <td>0.000</td>
      <td>0.604</td>
      <td>@BBC</td>
      <td>0.396</td>
      <td>'Good luck, little leatherback' - Sir David At...</td>
      <td>16</td>
    </tr>
    <tr>
      <th>17</th>
      <td>-0.1803</td>
      <td>0.213</td>
      <td>0.602</td>
      <td>@BBC</td>
      <td>0.185</td>
      <td>'There's a huge bomb up there. I'm 76% sure. I...</td>
      <td>17</td>
    </tr>
    <tr>
      <th>18</th>
      <td>0.6249</td>
      <td>0.000</td>
      <td>0.797</td>
      <td>@BBC</td>
      <td>0.203</td>
      <td>Miles of cable and thousands of twinkling bulb...</td>
      <td>18</td>
    </tr>
    <tr>
      <th>19</th>
      <td>0.5574</td>
      <td>0.000</td>
      <td>0.816</td>
      <td>@BBC</td>
      <td>0.184</td>
      <td>Tonight, David Attenborough investigates the r...</td>
      <td>19</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.5719</td>
      <td>0.000</td>
      <td>0.730</td>
      <td>@BBC</td>
      <td>0.270</td>
      <td>98-year-old Mary wins Christmas, and our heart...</td>
      <td>20</td>
    </tr>
    <tr>
      <th>21</th>
      <td>-0.2263</td>
      <td>0.137</td>
      <td>0.863</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>RT @BBCTwo: Who wants a Sneaky Peak at the nex...</td>
      <td>21</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.8765</td>
      <td>0.116</td>
      <td>0.555</td>
      <td>@BBC</td>
      <td>0.329</td>
      <td>RT @bbcstories: "I think people my age think t...</td>
      <td>22</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>📻 How to deal with mental health issues in the...</td>
      <td>23</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0.2500</td>
      <td>0.000</td>
      <td>0.900</td>
      <td>@BBC</td>
      <td>0.100</td>
      <td>This 12-year-old has created a cheap way to te...</td>
      <td>24</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.5279</td>
      <td>0.000</td>
      <td>0.860</td>
      <td>@BBC</td>
      <td>0.140</td>
      <td>📞👽 We've been listening for signs of life from...</td>
      <td>25</td>
    </tr>
    <tr>
      <th>26</th>
      <td>0.4588</td>
      <td>0.000</td>
      <td>0.864</td>
      <td>@BBC</td>
      <td>0.136</td>
      <td>Thousands of internet users have come together...</td>
      <td>26</td>
    </tr>
    <tr>
      <th>27</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>That was unexpected. 😂\n\nMichael McIntyre's B...</td>
      <td>27</td>
    </tr>
    <tr>
      <th>28</th>
      <td>0.3595</td>
      <td>0.000</td>
      <td>0.878</td>
      <td>@BBC</td>
      <td>0.122</td>
      <td>#SkiSunday is back! ⛷🎿 Join the team in Val D'...</td>
      <td>28</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@BBC</td>
      <td>0.000</td>
      <td>💰 @DanTDM has been named the highest-earning Y...</td>
      <td>29</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>470</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>The Power of Touch, Especially for Men https:/...</td>
      <td>70</td>
    </tr>
    <tr>
      <th>471</th>
      <td>-0.2960</td>
      <td>0.199</td>
      <td>0.663</td>
      <td>@nytimes</td>
      <td>0.138</td>
      <td>After the death of a young son, can a long roa...</td>
      <td>71</td>
    </tr>
    <tr>
      <th>472</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>Tens of thousands of people said goodbye and "...</td>
      <td>72</td>
    </tr>
    <tr>
      <th>473</th>
      <td>-0.5423</td>
      <td>0.280</td>
      <td>0.720</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>‘S.N.L.,’ Santa and James Franco Relentlessly ...</td>
      <td>73</td>
    </tr>
    <tr>
      <th>474</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>"To me, the college send-off was not a blues f...</td>
      <td>74</td>
    </tr>
    <tr>
      <th>475</th>
      <td>-0.5972</td>
      <td>0.246</td>
      <td>0.754</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>He defected from North Korea more than 3 decad...</td>
      <td>75</td>
    </tr>
    <tr>
      <th>476</th>
      <td>0.7351</td>
      <td>0.000</td>
      <td>0.733</td>
      <td>@nytimes</td>
      <td>0.267</td>
      <td>Ruby Rose has a whole closet dedicated to skin...</td>
      <td>76</td>
    </tr>
    <tr>
      <th>477</th>
      <td>0.0772</td>
      <td>0.000</td>
      <td>0.909</td>
      <td>@nytimes</td>
      <td>0.091</td>
      <td>Actually, you do want to know how this sausage...</td>
      <td>77</td>
    </tr>
    <tr>
      <th>478</th>
      <td>0.4404</td>
      <td>0.000</td>
      <td>0.707</td>
      <td>@nytimes</td>
      <td>0.293</td>
      <td>8 ways to have a better relationship in 2018 h...</td>
      <td>78</td>
    </tr>
    <tr>
      <th>479</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>An algorithm analyzed 52 million dolphin click...</td>
      <td>79</td>
    </tr>
    <tr>
      <th>480</th>
      <td>-0.5106</td>
      <td>0.202</td>
      <td>0.798</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>Older Venezuelans who have migrated say that a...</td>
      <td>80</td>
    </tr>
    <tr>
      <th>481</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>How to use an Instant Pot https://t.co/1nJIGi8...</td>
      <td>81</td>
    </tr>
    <tr>
      <th>482</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>Macron Steps Into Middle East Role as U.S. Ret...</td>
      <td>82</td>
    </tr>
    <tr>
      <th>483</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>These almond cookies need just 4 ingredients h...</td>
      <td>83</td>
    </tr>
    <tr>
      <th>484</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>It has been several generations since Haiti wa...</td>
      <td>84</td>
    </tr>
    <tr>
      <th>485</th>
      <td>0.8225</td>
      <td>0.000</td>
      <td>0.677</td>
      <td>@nytimes</td>
      <td>0.323</td>
      <td>Many athletes have been told that smiling whil...</td>
      <td>85</td>
    </tr>
    <tr>
      <th>486</th>
      <td>0.6369</td>
      <td>0.000</td>
      <td>0.588</td>
      <td>@nytimes</td>
      <td>0.412</td>
      <td>The best TV shows of 2017 https://t.co/b6UzsqJTb8</td>
      <td>86</td>
    </tr>
    <tr>
      <th>487</th>
      <td>-0.2500</td>
      <td>0.087</td>
      <td>0.913</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>Trump reduced the size of 2 monuments this wee...</td>
      <td>87</td>
    </tr>
    <tr>
      <th>488</th>
      <td>0.2023</td>
      <td>0.000</td>
      <td>0.909</td>
      <td>@nytimes</td>
      <td>0.091</td>
      <td>Here's how to get your body and mind in top sh...</td>
      <td>88</td>
    </tr>
    <tr>
      <th>489</th>
      <td>0.6369</td>
      <td>0.000</td>
      <td>0.826</td>
      <td>@nytimes</td>
      <td>0.174</td>
      <td>Maybe your love of “Star Wars” runs deep. Mayb...</td>
      <td>89</td>
    </tr>
    <tr>
      <th>490</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>26 casseroles for cold nights https://t.co/K2t...</td>
      <td>90</td>
    </tr>
    <tr>
      <th>491</th>
      <td>0.3818</td>
      <td>0.000</td>
      <td>0.867</td>
      <td>@nytimes</td>
      <td>0.133</td>
      <td>In “Home,” an entire house is custom built, ro...</td>
      <td>91</td>
    </tr>
    <tr>
      <th>492</th>
      <td>0.4939</td>
      <td>0.000</td>
      <td>0.849</td>
      <td>@nytimes</td>
      <td>0.151</td>
      <td>Here’s how to become part of the small group o...</td>
      <td>92</td>
    </tr>
    <tr>
      <th>493</th>
      <td>0.8481</td>
      <td>0.000</td>
      <td>0.685</td>
      <td>@nytimes</td>
      <td>0.315</td>
      <td>Come what may March 4, the night of the Academ...</td>
      <td>93</td>
    </tr>
    <tr>
      <th>494</th>
      <td>0.3400</td>
      <td>0.000</td>
      <td>0.888</td>
      <td>@nytimes</td>
      <td>0.112</td>
      <td>Permits to hunt bighorn sheep are auctioned fo...</td>
      <td>94</td>
    </tr>
    <tr>
      <th>495</th>
      <td>-0.3818</td>
      <td>0.120</td>
      <td>0.880</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>For other presidents, every day is a test of h...</td>
      <td>95</td>
    </tr>
    <tr>
      <th>496</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>Heisman Trophy Goes to Baker Mayfield, Explosi...</td>
      <td>96</td>
    </tr>
    <tr>
      <th>497</th>
      <td>0.3182</td>
      <td>0.000</td>
      <td>0.887</td>
      <td>@nytimes</td>
      <td>0.113</td>
      <td>Remember Emma Stone’s cringe-worthy one-woman ...</td>
      <td>97</td>
    </tr>
    <tr>
      <th>498</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>Do not microwave an egg. https://t.co/yeALzNGtnc</td>
      <td>98</td>
    </tr>
    <tr>
      <th>499</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>“You hit a patch of ice on a turn, that’s it,”...</td>
      <td>99</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 7 columns</p>
</div>




```python
# Get overall/average of sentiment compount values for each agency
avg_sentiments_df = sentiments_df.groupby('News Organization').mean()
avg_sentiments_df.to_csv('avgsentiments.csv')
avg_sentiments_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Compound Score</th>
      <th>Negative Score</th>
      <th>Neutral Score</th>
      <th>Positive Score</th>
      <th>Tweet Number</th>
    </tr>
    <tr>
      <th>News Organization</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>@BBC</th>
      <td>0.170004</td>
      <td>0.04050</td>
      <td>0.85452</td>
      <td>0.10496</td>
      <td>49.5</td>
    </tr>
    <tr>
      <th>@CBS</th>
      <td>0.374197</td>
      <td>0.00980</td>
      <td>0.83015</td>
      <td>0.16005</td>
      <td>49.5</td>
    </tr>
    <tr>
      <th>@CNN</th>
      <td>-0.068466</td>
      <td>0.08498</td>
      <td>0.86150</td>
      <td>0.05352</td>
      <td>49.5</td>
    </tr>
    <tr>
      <th>@FoxNews</th>
      <td>0.022076</td>
      <td>0.09076</td>
      <td>0.80606</td>
      <td>0.10318</td>
      <td>49.5</td>
    </tr>
    <tr>
      <th>@nytimes</th>
      <td>0.032135</td>
      <td>0.07532</td>
      <td>0.83807</td>
      <td>0.08660</td>
      <td>49.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get timestamp
present_date = time.strftime("%m/%d/%Y")
present_date
```




    '12/10/2017'




```python
# Create dataframe for each news source and plot
bbc_compound_df = sentiments_df.loc[sentiments_df['News Organization'] == '@BBC']
plt.scatter(bbc_compound_df['Tweet Number'], bbc_compound_df['Compound Score'], label='BBC')

cbs_compound_df = sentiments_df.loc[sentiments_df['News Organization'] == '@CBS']
plt.scatter(cbs_compound_df['Tweet Number'], cbs_compound_df['Compound Score'], label='CBS')

cnn_compound_df = sentiments_df.loc[sentiments_df['News Organization'] == '@CNN']
plt.scatter(cnn_compound_df['Tweet Number'], cnn_compound_df['Compound Score'], label='CNN')

foxnews_compound_df = sentiments_df.loc[sentiments_df['News Organization'] == '@FoxNews']
plt.scatter(foxnews_compound_df['Tweet Number'], foxnews_compound_df['Compound Score'], label='Fox News')

nytimes_compound_df = sentiments_df.loc[sentiments_df['News Organization'] == '@nytimes']
plt.scatter(nytimes_compound_df['Tweet Number'], nytimes_compound_df['Compound Score'], label='NY Times')

plt.title(f'Sentiment Analysis of Media Tweets {present_date}')
plt.xlabel('Tweets Ago')
plt.ylabel('Tweet Popularity')
plt.legend(loc='center left', bbox_to_anchor=(1, 0.5))
plt.grid(True)
plt.savefig('CompoundNewsMedia.png')
plt.show()
```


![png](output_8_0.png)



```python
# Plot average sentiment compound of each source
# Reset index
updated_sentiments_df = avg_sentiments_df.reset_index()

plt.bar(updated_sentiments_df['News Organization'], updated_sentiments_df['Compound Score'])
plt.title(f'Overall Media Sentiment based on Twitter {present_date}')
plt.xlabel('News Organization')
plt.ylabel('Tweet Popularity')
plt.savefig('AvgCompoundNewsMedia.png')
plt.show()
```


![png](output_9_0.png)


ANALYSIS
1. Overall, from the bar graph, CNN is the most negative of the group of news organizations.
2. CBS is the most positive as we can see from both the scatter plot and bar graph.
3. NY Times looks like its the most neutral with many points located at 0 for tweet popularity.

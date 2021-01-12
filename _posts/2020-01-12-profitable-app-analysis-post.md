---
layout: post
title: "How To Develop A Profitable App"
excerpt: "A simple analysis to shed light on App Store and Google Store apps."
categories: [python]
comments: true
---

If you decide to develop a mobile application now, what makes it profitable? Following quick analysis will not give a clear answer to this question of course, but I am going to explore two data sets from App Store and Google Store in which we can discover most popular apps and app categories using very simple data analysis functionalities of python.

_Please note that this is a guided analysis that I practiced on [dataquest.io](https://www.dataquest.io). It's great MOOC that we've been using @TIER to keep our data analytics & engineering muscles warm._



```python
# Requirements
from csv import reader
```

## Data sets

There are two data sets from [kaggle.com](https://www.kaggle.com) that seem useful to explore mobile apps in main stores: 

- [Google Play Store Apps](https://www.kaggle.com/lava18/google-play-store-apps)

- [Mobile App Store (7200 apps)](https://www.kaggle.com/ramamet4/app-store-apple-data-set-10k-apps)

Data sets contain data about approximately 10k and 7k mobile apps from Google Play Store and Apple Store, respectively. Feel free to take a look at Kaggle pages to get better understanding of data sets.



```python
### The Google Play data set ###

open_file = open('googleplaystore.csv')
read_file = reader(open_file)
android = list(read_file)
android_header = android[0]
android_data = android[1:]

### The App Store data set ###
open_file = open('AppleStore.csv')
read_file = reader(open_file)
ios = list(read_file)
ios_header = ios[0]
ios_data = ios[1:]
```


```python
def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line between rows
        
    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))

print(android_header)

explore_data(android_data, 0, 3, True)
```

    ['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']
    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['Coloring book moana', 'ART_AND_DESIGN', '3.9', '967', '14M', '500,000+', 'Free', '0', 'Everyone', 'Art & Design;Pretend Play', 'January 15, 2018', '2.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    Number of rows: 10841
    Number of columns: 13



```python
def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line between rows
        
    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))

print(ios_header)

explore_data(ios_data, 0, 3, True)
```

    ['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']
    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    ['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']
    
    
    Number of rows: 7197
    Number of columns: 16


## Clean your data sets before playing with them

We need to do some data cleaning before starting exploration. If you are using a dataset shared on Kaggle.com, always check the discussion to see what other data enthusiasts found worth to share or ask about the subject data. It both saves incredible amount of time and also gives a better understanding of challenges that are waiting for you. For instance, one of the fellas has addressed [a wrong entry](https://www.kaggle.com/lava18/google-play-store-apps/discussion/66015), which you might want to not include in your analysis.

### Missing and incorrect records

Based on the discussions on kaggle.com, the following entity's  `Category` value is missing so I'm deleting the entity.


```python
print(android_data[10472])
```

    ['Life Made WI-Fi Touchscreen Photo Frame', '1.9', '19', '3.0M', '1,000+', 'Free', '0', 'Everyone', '', 'February 11, 2018', '1.0.19', '4.0 and up']



```python
del android_data[10472]
```

### Duplicate Entities

The iOS data set seems cleaner, but there are few rows that might be confusing in case of another analysis. Below entities are four different applications, but each pair has the same name. I don't mind to keep them in the data set, since they won't mislead our argument around profitability analysis.

| App Name            | Content Rating | Rating | Rating Count | App Size | Version No. |
|---------------------|----------------|--------|--------------|----------|-------------|
| Mannequin Challenge | 9+             | 3.0    | 668          | 109 MB   | 1.4         |
| Mannequin Challenge | 4+             | 4.0    | 105          | 59 MB    | 1.0.1       |
| VR Roller Coaster   | 4+             | 3.5    | 107          | 169 MB   | 2.0.0       |
| VR Roller Coaster   | 4+             | 3.5    | 67           | 241 MB   | 0.8.1       |



```python
duplicate_apps = []
unique_apps = []

for app in android_data:
    name = app[0]
    if name in unique_apps:
        duplicate_apps.append(name)
    else:
        unique_apps.append(name)
    
duplicate_apps_ios = []
unique_apps_ios = []

for app in ios_data:
    name = app[0]
    if name in unique_apps:
        duplicate_apps.append(name)
    else:
        unique_apps.append(name)
    
print('Number of duplicate iOS apps:', len(duplicate_apps_ios))
print('Number of duplicate android apps:', len(duplicate_apps))
print('\n')
print('Examples of duplicate android apps:', duplicate_apps[:15])
```

    Number of duplicate iOS apps: 0
    Number of duplicate android apps: 1181
    
    
    Examples of duplicate android apps: ['Quick PDF Scanner + OCR FREE', 'Box', 'Google My Business', 'ZOOM Cloud Meetings', 'join.me - Simple Meetings', 'Box', 'Zenefits', 'Google Ads', 'Google My Business', 'Slack', 'FreshBooks Classic', 'Insightly CRM', 'QuickBooks Accounting: Invoicing & Expenses', 'HipChat - Chat Built for Teams', 'Xero Accounting Software']


Android data set contains 1181 duplicate application names in total, while there is no duplicate entity in iOS data set except the 2 examples that were discussed before.   

One example of duplicate entity is Instagram, which exists for 4 times. The notable difference among etities is the amount of app reviews which gives us an idea about the recency of the records. We can keep the entity with the highes value on app review amounts to keep the data set up-to-date.


```python
for app in android_data:
    app_name = app[0]
    if app_name == 'Instagram':
        print(app)
```

    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577446', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66509917', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']


First, I am going to create a dictionary with each app and maximum review number recorded in data set. Then, `android_clean` data set will include only entities with maximum review amounts.


```python
reviews_max = {}

for app in android_data:
    app_name = app[0]
    n_reviews = float(app[3])

    if app_name in reviews_max and reviews_max[app_name] < n_reviews:
        reviews_max[app_name] = n_reviews
    
    elif app_name not in reviews_max:
        reviews_max[app_name] = n_reviews

print(len(reviews_max))

android_clean = []
already_added = []

for app in android_data:
    app_name  = app[0]
    n_reviews = float(app[3])
    
    if (reviews_max[app_name] == n_reviews) and (app_name not in already_added):
        android_clean.append(app)
        already_added.append(app_name)

print(len(android_clean))
```

    9659
    9659


### Non-English apps

In addition to duplicate entries, another subgroup of apps that we are going to exclude from analysis is the ones that target non-English speaking users. As a short cut, I am going to check the application names with non-ASCII characters which consist of English alphabet, digits, punctuation marks and other symbols. Each ASCII character has a corresponding number between 0 and 127 associated with it, and we can take advantage of that to build a function that checks an app name and tells us whether it contains non-ASCII characters. Simply, below function will detect whether the app name includes non-ASCII character or not:


```python
def is_english(string):
    
    for character in string:
        if ord(character) > 127:
            return False
    
    return True

print(is_english('Instagram'))
print(is_english('爱奇艺PPS -《欢乐颂2》电视剧热播'))
```

    True
    False


Even though the function works well, we might filter out some apps
with non-ASCII characters in name, i.e, smileys.


```python
print(is_english('Docs To Go™ Free Office Suite'))
print(is_english('Instachat 😜'))

print(ord('™'))
print(ord('😜'))
```

    False
    False
    8482
    128540


To minimize this impact, I am going to modify function to filter out apps if there are more than 3 non-ASCII characters:


```python
def is_english(string):
    non_ascii = 0
    
    for character in string:
        if ord(character) > 127:
            non_ascii += 1
    
    if non_ascii > 3:
        return False
    else:
        return True

print(is_english('Docs To Go™ Free Office Suite'))
print(is_english('Instachat 😜'))
```

    True
    True


Let's use this function as a filter on both data sets


```python
android_english = []
ios_english = []

for app in android_clean:
    app_name = app[0]
    if is_english(app_name):
        android_english.append(app)
        
for app in ios:
    app_name = app[1]
    if is_english(app_name):
        ios_english.append(app)
        
explore_data(android_english, 0, 3, True)
print('\n')
explore_data(ios_english, 0, 3, True)
```

    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    Number of rows: 9614
    Number of columns: 13
    
    
    ['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']
    
    
    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    Number of rows: 6184
    Number of columns: 16


### Filter out paid apps from each data set

Since I want to explore free apps only, I am also filtering out apps with a purchase price.


```python
android_free = []

for app in android_english:
    price = app[7]
    
    if price == "0":
        android_free.append(app)
        
len(android_free)
```




    8864




```python
ios_free = []

for app in ios_english:
    price = app[4]
    
    if price == "0.0":
        ios_free.append(app)
        

len(ios_free)
```




    3222



## The Most Common App Categories in Each Store


```python
def freq_table(dataset, index):
    table = {}
    total = 0
    
    for row in dataset:
        total += 1
        value = row[index]
        if value in table:
            table[value] += 1
        else:
            table[value] = 1
    
    table_percentages = {}
    for key in table:
        percentage = (table[key] / total) * 100
        table_percentages[key] = percentage 
    
    return table_percentages


def display_table(dataset, index):
    table = freq_table(dataset, index)
    table_display = []
    for key in table:
        key_val_as_tuple = (table[key], key)
        table_display.append(key_val_as_tuple)
        
    table_sorted = sorted(table_display, reverse = True)
    for entry in table_sorted:
        print(entry[1], ':', entry[0])
```


```python
ios_frequencies = freq_table(ios_free, 11)
android_freq_category = freq_table(android_free, 1)
android_freq_genre = freq_table(android_free, 9)
```


```python
display_table(ios_free,11)
```

    Games : 58.16263190564867
    Entertainment : 7.883302296710118
    Photo & Video : 4.9658597144630665
    Education : 3.662321539416512
    Social Networking : 3.2898820608317814
    Shopping : 2.60707635009311
    Utilities : 2.5139664804469275
    Sports : 2.1415270018621975
    Music : 2.0484171322160147
    Health & Fitness : 2.0173805090006205
    Productivity : 1.7380509000620732
    Lifestyle : 1.5828677839851024
    News : 1.3345747982619491
    Travel : 1.2414649286157666
    Finance : 1.1173184357541899
    Weather : 0.8690254500310366
    Food & Drink : 0.8069522036002483
    Reference : 0.5586592178770949
    Business : 0.5276225946617008
    Book : 0.4345127250155183
    Navigation : 0.186219739292365
    Medical : 0.186219739292365
    Catalogs : 0.12414649286157665


Among all free English apps, more than a half of them are games. Next most common genre is entertainment which is almost 8% of the applications in App Store. Entertainment is followed by photo & video apps that is close to 5%. Broadly the App Store is dominated by the apps that are designed for fun where apps with more practical purposes (education, productivity, shopping, etc.) are more rare. However, it does not necessarily mean that the demand is as high as what is offered on App Store. High number of game applicatons might be used by low number of users.


```python
display_table(android_free,1)
```

    FAMILY : 18.907942238267147
    GAME : 9.724729241877256
    TOOLS : 8.461191335740072
    BUSINESS : 4.591606498194946
    LIFESTYLE : 3.9034296028880866
    PRODUCTIVITY : 3.892148014440433
    FINANCE : 3.7003610108303246
    MEDICAL : 3.531137184115524
    SPORTS : 3.395758122743682
    PERSONALIZATION : 3.3167870036101084
    COMMUNICATION : 3.2378158844765346
    HEALTH_AND_FITNESS : 3.0798736462093865
    PHOTOGRAPHY : 2.944494584837545
    NEWS_AND_MAGAZINES : 2.7978339350180503
    SOCIAL : 2.6624548736462095
    TRAVEL_AND_LOCAL : 2.33528880866426
    SHOPPING : 2.2450361010830324
    BOOKS_AND_REFERENCE : 2.1435018050541514
    DATING : 1.861462093862816
    VIDEO_PLAYERS : 1.7937725631768955
    MAPS_AND_NAVIGATION : 1.3989169675090252
    FOOD_AND_DRINK : 1.2409747292418771
    EDUCATION : 1.1620036101083033
    ENTERTAINMENT : 0.9589350180505415
    LIBRARIES_AND_DEMO : 0.9363718411552346
    AUTO_AND_VEHICLES : 0.9250902527075812
    HOUSE_AND_HOME : 0.8235559566787004
    WEATHER : 0.8009927797833934
    EVENTS : 0.7107400722021661
    PARENTING : 0.6543321299638989
    ART_AND_DESIGN : 0.6430505415162455
    COMICS : 0.6204873646209386
    BEAUTY : 0.5979241877256317


On the other hand, Google Play seems different at first glance. There are considerably high number of family, business, tools, lifestyle, productivity apps. Considering that family category apps are mostly games for kids, total number of games still seems higher than other categories. 

Even so, practical apps seem to have a better representation on Google Play compared to App Store. This is also confirmed by the frequency table of app genres:


```python
display_table(android_free,9)
```

    Tools : 8.449909747292418
    Entertainment : 6.069494584837545
    Education : 5.347472924187725
    Business : 4.591606498194946
    Productivity : 3.892148014440433
    Lifestyle : 3.892148014440433
    Finance : 3.7003610108303246
    Medical : 3.531137184115524
    Sports : 3.463447653429603
    Personalization : 3.3167870036101084
    Communication : 3.2378158844765346
    Action : 3.1024368231046933
    Health & Fitness : 3.0798736462093865
    Photography : 2.944494584837545
    News & Magazines : 2.7978339350180503
    Social : 2.6624548736462095
    Travel & Local : 2.3240072202166067
    Shopping : 2.2450361010830324
    Books & Reference : 2.1435018050541514
    Simulation : 2.0419675090252705
    Dating : 1.861462093862816
    Arcade : 1.8501805054151623
    Video Players & Editors : 1.7712093862815883
    Casual : 1.7599277978339352
    Maps & Navigation : 1.3989169675090252
    Food & Drink : 1.2409747292418771
    Puzzle : 1.128158844765343
    Racing : 0.9927797833935018
    Role Playing : 0.9363718411552346
    Libraries & Demo : 0.9363718411552346
    Auto & Vehicles : 0.9250902527075812
    Strategy : 0.9138086642599278
    House & Home : 0.8235559566787004
    Weather : 0.8009927797833934
    Events : 0.7107400722021661
    Adventure : 0.6768953068592057
    Comics : 0.6092057761732852
    Beauty : 0.5979241877256317
    Art & Design : 0.5979241877256317
    Parenting : 0.4963898916967509
    Card : 0.45126353790613716
    Casino : 0.42870036101083037
    Trivia : 0.41741877256317694
    Educational;Education : 0.39485559566787
    Board : 0.3835740072202166
    Educational : 0.3722924187725632
    Education;Education : 0.33844765342960287
    Word : 0.2594765342960289
    Casual;Pretend Play : 0.236913357400722
    Music : 0.2030685920577617
    Racing;Action & Adventure : 0.16922382671480143
    Puzzle;Brain Games : 0.16922382671480143
    Entertainment;Music & Video : 0.16922382671480143
    Casual;Brain Games : 0.13537906137184114
    Casual;Action & Adventure : 0.13537906137184114
    Arcade;Action & Adventure : 0.12409747292418773
    Action;Action & Adventure : 0.10153429602888085
    Educational;Pretend Play : 0.09025270758122744
    Simulation;Action & Adventure : 0.078971119133574
    Parenting;Education : 0.078971119133574
    Entertainment;Brain Games : 0.078971119133574
    Board;Brain Games : 0.078971119133574
    Parenting;Music & Video : 0.06768953068592057
    Educational;Brain Games : 0.06768953068592057
    Casual;Creativity : 0.06768953068592057
    Art & Design;Creativity : 0.06768953068592057
    Education;Pretend Play : 0.056407942238267145
    Role Playing;Pretend Play : 0.04512635379061372
    Education;Creativity : 0.04512635379061372
    Role Playing;Action & Adventure : 0.033844765342960284
    Puzzle;Action & Adventure : 0.033844765342960284
    Entertainment;Creativity : 0.033844765342960284
    Entertainment;Action & Adventure : 0.033844765342960284
    Educational;Creativity : 0.033844765342960284
    Educational;Action & Adventure : 0.033844765342960284
    Education;Music & Video : 0.033844765342960284
    Education;Brain Games : 0.033844765342960284
    Education;Action & Adventure : 0.033844765342960284
    Adventure;Action & Adventure : 0.033844765342960284
    Video Players & Editors;Music & Video : 0.02256317689530686
    Sports;Action & Adventure : 0.02256317689530686
    Simulation;Pretend Play : 0.02256317689530686
    Puzzle;Creativity : 0.02256317689530686
    Music;Music & Video : 0.02256317689530686
    Entertainment;Pretend Play : 0.02256317689530686
    Casual;Education : 0.02256317689530686
    Board;Action & Adventure : 0.02256317689530686
    Video Players & Editors;Creativity : 0.01128158844765343
    Trivia;Education : 0.01128158844765343
    Travel & Local;Action & Adventure : 0.01128158844765343
    Tools;Education : 0.01128158844765343
    Strategy;Education : 0.01128158844765343
    Strategy;Creativity : 0.01128158844765343
    Strategy;Action & Adventure : 0.01128158844765343
    Simulation;Education : 0.01128158844765343
    Role Playing;Brain Games : 0.01128158844765343
    Racing;Pretend Play : 0.01128158844765343
    Puzzle;Education : 0.01128158844765343
    Parenting;Brain Games : 0.01128158844765343
    Music & Audio;Music & Video : 0.01128158844765343
    Lifestyle;Pretend Play : 0.01128158844765343
    Lifestyle;Education : 0.01128158844765343
    Health & Fitness;Education : 0.01128158844765343
    Health & Fitness;Action & Adventure : 0.01128158844765343
    Entertainment;Education : 0.01128158844765343
    Communication;Creativity : 0.01128158844765343
    Comics;Creativity : 0.01128158844765343
    Casual;Music & Video : 0.01128158844765343
    Card;Action & Adventure : 0.01128158844765343
    Books & Reference;Education : 0.01128158844765343
    Art & Design;Pretend Play : 0.01128158844765343
    Art & Design;Action & Adventure : 0.01128158844765343
    Arcade;Pretend Play : 0.01128158844765343
    Adventure;Education : 0.01128158844765343


The genre is a much more granular categorisation compared to Category column which breaks down each category of apps. I'll be using category column moving forward. Based on my expression up to this point, while App Store is mainly dominated by apps designed for fun, Google Play shows a more balanced picture landscape of both for-fun and practical apps. Next, I'll look into the number of users of each category.

## Most Popular Apps by Genre on App Store


```python
genres_ios = freq_table(ios_free, 11)

for genre in genres_ios:
    total = 0
    len_genre = 0
    for app in ios_free:
        genre_app = app[-5]
        if genre_app == genre:            
            n_ratings = float(app[5])
            total += n_ratings
            len_genre += 1
    avg_n_ratings = total / len_genre
    print(genre, ':', avg_n_ratings)
```

    Photo & Video : 28441.54375
    Finance : 31467.944444444445
    Medical : 612.0
    Shopping : 26919.690476190477
    Music : 57326.530303030304
    Education : 7003.983050847458
    Lifestyle : 16485.764705882353
    Catalogs : 4004.0
    Sports : 23008.898550724636
    Navigation : 86090.33333333333
    Utilities : 18684.456790123455
    Food & Drink : 33333.92307692308
    Business : 7491.117647058823
    Health & Fitness : 23298.015384615384
    Entertainment : 14029.830708661417
    Productivity : 21028.410714285714
    Reference : 74942.11111111111
    Social Networking : 71548.34905660378
    Book : 39758.5
    Games : 22788.6696905016
    News : 21248.023255813954
    Travel : 28243.8
    Weather : 52279.892857142855


There is no data showing the total number of installs in App Store data set so we're using the average number of reviews per each category as a proxy. Based on the average review amounts, Navigation, Social Networks and Reference categories receive significantly high number of reviews. This is most probably driven by few number of highly used apps under these categories.


```python
for app in ios_free:
    if app[11] == 'Navigation':
        print(app[1], ':', app[5]) # print name and number of ratings
```

    Waze - GPS Navigation, Maps & Real-time Traffic : 345046
    Google Maps - Navigation & Transit : 154911
    Geocaching® : 12811
    CoPilot GPS – Car Navigation & Offline Maps : 3582
    ImmobilienScout24: Real Estate Search in Germany : 187
    Railway Route Search : 5


Almost all of the reviews in Navigation genre is for Waze and Google Maps apps. Similarly applications like Facebook and Pinterest have high number of reviews which skewes up the genre average.


```python
for app in ios_free:
    if app[11] == 'Social Networking':
        print(app[1], ':', app[5]) # print name and number of ratings
```

    Facebook : 2974676
    Pinterest : 1061624
    Skype for iPhone : 373519
    Messenger : 351466
    Tumblr : 334293
    WhatsApp Messenger : 287589
    Kik : 260965
    ooVoo – Free Video Call, Text and Voice : 177501
    TextNow - Unlimited Text + Calls : 164963
    Viber Messenger – Text & Call : 164249
    Followers - Social Analytics For Instagram : 112778
    MeetMe - Chat and Meet New People : 97072
    We Heart It - Fashion, wallpapers, quotes, tattoos : 90414
    InsTrack for Instagram - Analytics Plus More : 85535
    Tango - Free Video Call, Voice and Chat : 75412
    LinkedIn : 71856
    Match™ - #1 Dating App. : 60659
    Skype for iPad : 60163
    POF - Best Dating App for Conversations : 52642
    Timehop : 49510
    Find My Family, Friends & iPhone - Life360 Locator : 43877
    Whisper - Share, Express, Meet : 39819
    Hangouts : 36404
    LINE PLAY - Your Avatar World : 34677
    WeChat : 34584
    Badoo - Meet New People, Chat, Socialize. : 34428
    Followers + for Instagram - Follower Analytics : 28633
    GroupMe : 28260
    Marco Polo Video Walkie Talkie : 27662
    Miitomo : 23965
    SimSimi : 23530
    Grindr - Gay and same sex guys chat, meet and date : 23201
    Wishbone - Compare Anything : 20649
    imo video calls and chat : 18841
    After School - Funny Anonymous School News : 18482
    Quick Reposter - Repost, Regram and Reshare Photos : 17694
    Weibo HD : 16772
    Repost for Instagram : 15185
    Live.me – Live Video Chat & Make Friends Nearby : 14724
    Nextdoor : 14402
    Followers Analytics for Instagram - InstaReport : 13914
    YouNow: Live Stream Video Chat : 12079
    FollowMeter for Instagram - Followers Tracking : 11976
    LINE : 11437
    eHarmony™ Dating App - Meet Singles : 11124
    Discord - Chat for Gamers : 9152
    QQ : 9109
    Telegram Messenger : 7573
    Weibo : 7265
    Periscope - Live Video Streaming Around the World : 6062
    Chat for Whatsapp - iPad Version : 5060
    QQ HD : 5058
    Followers Analysis Tool For Instagram App Free : 4253
    live.ly - live video streaming : 4145
    Houseparty - Group Video Chat : 3991
    SOMA Messenger : 3232
    Monkey : 3060
    Down To Lunch : 2535
    Flinch - Video Chat Staring Contest : 2134
    Highrise - Your Avatar Community : 2011
    LOVOO - Dating Chat : 1985
    PlayStation®Messages : 1918
    BOO! - Video chat camera with filters & stickers : 1805
    Qzone : 1649
    Chatous - Chat with new people : 1609
    Kiwi - Q&A : 1538
    GhostCodes - a discovery app for Snapchat : 1313
    Jodel : 1193
    FireChat : 1037
    Google Duo - simple video calling : 1033
    Fiesta by Tango - Chat & Meet New People : 885
    Google Allo — smart messaging : 862
    Peach — share vividly : 727
    Hey! VINA - Where Women Meet New Friends : 719
    Battlefield™ Companion : 689
    All Devices for WhatsApp - Messenger for iPad : 682
    Chat for Pokemon Go - GoChat : 500
    IAmNaughty – Dating App to Meet New People Online : 463
    Qzone HD : 458
    Zenly - Locate your friends in realtime : 427
    League of Legends Friends : 420
    豆瓣 : 407
    Candid - Speak Your Mind Freely : 398
    知乎 : 397
    Selfeo : 366
    Fake-A-Location Free ™ : 354
    Popcorn Buzz - Free Group Calls : 281
    Fam — Group video calling for iMessage : 279
    QQ International : 274
    Ameba : 269
    SoundCloud Pulse: for creators : 240
    Tantan : 235
    Cougar Dating & Life Style App for Mature Women : 213
    Rawr Messenger - Dab your chat : 180
    WhenToPost: Best Time to Post Photos for Instagram : 158
    Inke—Broadcast an amazing life : 147
    Mustknow - anonymous video Q&A : 53
    CTFxCmoji : 39
    Lobi : 36
    Chain: Collaborate On MyVideo Story/Group Video : 35
    botman - Real time video chat : 7
    BestieBox : 0
    MATCH ON LINE chat : 0
    niconico ch : 0
    LINE BLOG : 0
    bit-tube - Live Stream Video Chat : 0


Reference apps alo have a high number of ratings on average, which is highly influenced by Bible and Dictionary.com apps. Apart from these two giant apps, the competition is relatively weaker than other categories.


```python
for app in ios_free:
    if app[11] == 'Reference':
        print(app[1], ':', app[5]) # print name and number of ratings
```

    Bible : 985920
    Dictionary.com Dictionary & Thesaurus : 200047
    Dictionary.com Dictionary & Thesaurus for iPad : 54175
    Google Translate : 26786
    Muslim Pro: Ramadan 2017 Prayer Times, Azan, Quran : 18418
    New Furniture Mods - Pocket Wiki & Game Tools for Minecraft PC Edition : 17588
    Merriam-Webster Dictionary : 16849
    Night Sky : 12122
    City Maps for Minecraft PE - The Best Maps for Minecraft Pocket Edition (MCPE) : 8535
    LUCKY BLOCK MOD ™ for Minecraft PC Edition - The Best Pocket Wiki & Mods Installer Tools : 4693
    GUNS MODS for Minecraft PC Edition - Mods Tools : 1497
    Guides for Pokémon GO - Pokemon GO News and Cheats : 826
    WWDC : 762
    Horror Maps for Minecraft PE - Download The Scariest Maps for Minecraft Pocket Edition (MCPE) Free : 718
    VPN Express : 14
    Real Bike Traffic Rider Virtual Reality Glasses : 8
    教えて!goo : 0
    Jishokun-Japanese English Dictionary & Translator : 0


### Most Popular Apps by Genre on Google Play

Google Play data set includes information regarding number of installs. However, the column consists of non-equal "bins" instead of precise download numbers. For instance, an application with 1M downloads and 4.9M downloads are in the same cluster. Still, we're going to use these numbers to have a general overview of installs per category on average.


```python
display_table(android_free, 5) # the Installs columns
```

    1,000,000+ : 15.726534296028879
    100,000+ : 11.552346570397113
    10,000,000+ : 10.548285198555957
    10,000+ : 10.198555956678701
    1,000+ : 8.393501805054152
    100+ : 6.915613718411552
    5,000,000+ : 6.825361010830325
    500,000+ : 5.561823104693141
    50,000+ : 4.7721119133574
    5,000+ : 4.512635379061372
    10+ : 3.5424187725631766
    500+ : 3.2490974729241873
    50,000,000+ : 2.3014440433213
    100,000,000+ : 2.1322202166064983
    50+ : 1.917870036101083
    5+ : 0.78971119133574
    1+ : 0.5076714801444043
    500,000,000+ : 0.2707581227436823
    1,000,000,000+ : 0.22563176895306858
    0+ : 0.04512635379061372
    0 : 0.01128158844765343


I am calculating the average by each category using these binned numbers. Similar to App Store, the very high averages of some categories are mainly driven by giant apps that very high number of users install such as Whatsapp and Skype under Communication category which have +1 billion downloads.


```python
categories_android = freq_table(android_free, 1)

for category in categories_android:
    total = 0
    len_category = 0
    for app in android_free:
        category_app = app[1]
        if category_app == category:            
            n_installs = app[5]
            n_installs = n_installs.replace(',', '')
            n_installs = n_installs.replace('+', '')
            total += float(n_installs)
            len_category += 1
    avg_n_installs = total / len_category
    print(category, ':', avg_n_installs)
```

    BOOKS_AND_REFERENCE : 8767811.894736841
    LIFESTYLE : 1437816.2687861272
    FINANCE : 1387692.475609756
    BUSINESS : 1712290.1474201474
    SHOPPING : 7036877.311557789
    WEATHER : 5074486.197183099
    AUTO_AND_VEHICLES : 647317.8170731707
    FOOD_AND_DRINK : 1924897.7363636363
    VIDEO_PLAYERS : 24727872.452830188
    ENTERTAINMENT : 11640705.88235294
    SOCIAL : 23253652.127118643
    COMMUNICATION : 38456119.167247385
    ART_AND_DESIGN : 1986335.0877192982
    TOOLS : 10801391.298666667
    SPORTS : 3638640.1428571427
    PRODUCTIVITY : 16787331.344927534
    BEAUTY : 513151.88679245283
    LIBRARIES_AND_DEMO : 638503.734939759
    EDUCATION : 1833495.145631068
    PARENTING : 542603.6206896552
    FAMILY : 3695641.8198090694
    TRAVEL_AND_LOCAL : 13984077.710144928
    HEALTH_AND_FITNESS : 4188821.9853479853
    HOUSE_AND_HOME : 1331540.5616438356
    PERSONALIZATION : 5201482.6122448975
    DATING : 854028.8303030303
    GAME : 15588015.603248259
    COMICS : 817657.2727272727
    MAPS_AND_NAVIGATION : 4056941.7741935486
    MEDICAL : 120550.61980830671
    NEWS_AND_MAGAZINES : 9549178.467741935
    EVENTS : 253542.22222222222
    PHOTOGRAPHY : 17840110.40229885



```python
for app in android_free:
    if app[1] == 'COMMUNICATION' and (app[5] == '1,000,000,000+'
                                      or app[5] == '500,000,000+'
                                      or app[5] == '100,000,000+'):
        print(app[0], ':', app[5])
```

    WhatsApp Messenger : 1,000,000,000+
    imo beta free calls and text : 100,000,000+
    Android Messages : 100,000,000+
    Google Duo - High Quality Video Calls : 500,000,000+
    Messenger – Text and Video Chat for Free : 1,000,000,000+
    imo free video calls and chat : 500,000,000+
    Skype - free IM & video calls : 1,000,000,000+
    Who : 100,000,000+
    GO SMS Pro - Messenger, Free Themes, Emoji : 100,000,000+
    LINE: Free Calls & Messages : 500,000,000+
    Google Chrome: Fast & Secure : 1,000,000,000+
    Firefox Browser fast & private : 100,000,000+
    UC Browser - Fast Download Private & Secure : 500,000,000+
    Gmail : 1,000,000,000+
    Hangouts : 1,000,000,000+
    Messenger Lite: Free Calls & Messages : 100,000,000+
    Kik : 100,000,000+
    KakaoTalk: Free Calls & Text : 100,000,000+
    Opera Mini - fast web browser : 100,000,000+
    Opera Browser: Fast and Secure : 100,000,000+
    Telegram : 100,000,000+
    Truecaller: Caller ID, SMS spam blocking & Dialer : 100,000,000+
    UC Browser Mini -Tiny Fast Private & Secure : 100,000,000+
    Viber Messenger : 500,000,000+
    WeChat : 100,000,000+
    Yahoo Mail – Stay Organized : 100,000,000+
    BBM - Free Calls & Messages : 100,000,000+


If we exclude this applications from the average calculation, it drops to  approximately 10%. This trend is the same for other categories such as video players where the market is dominated by apps like Youtube, Google Play Movies, etc. Or the social media giants like Facebook and Instagram pushes the average number of users really high and it's highly challinging to compete with these giants. 


```python

under_100_m = []

for app in android_free:
    n_installs = app[5]
    n_installs = n_installs.replace(',', '')
    n_installs = n_installs.replace('+', '')
    if (app[1] == 'COMMUNICATION') and (float(n_installs) < 100000000):
        under_100_m.append(float(n_installs))
        
sum(under_100_m) / len(under_100_m)
```




    3603485.3884615386



Getting back to the main purpose of analysis, if the main concern is publishing an app on markets, these categories don't seem to be comfortable domains to survive with profit, since they are dominated by a few giants who are very hard top compete against.

On the other hand, game market seems a bit saturated such that it's very likely to get lost among thousands of apps. Apart from these categories,   books and reference genre looks fairly popular as well, with an average number of installs of more than 8.7 million. It is also one of the fairly popular genres across genres in iOS and Android stores.

Let's take a look at some of the apps from this genre and their number of installs:


```python
for app in android_free:
    if app[1] == 'BOOKS_AND_REFERENCE':
        print(app[0], ':', app[5])
```

    E-Book Read - Read Book for free : 50,000+
    Download free book with green book : 100,000+
    Wikipedia : 10,000,000+
    Cool Reader : 10,000,000+
    Free Panda Radio Music : 100,000+
    Book store : 1,000,000+
    FBReader: Favorite Book Reader : 10,000,000+
    English Grammar Complete Handbook : 500,000+
    Free Books - Spirit Fanfiction and Stories : 1,000,000+
    Google Play Books : 1,000,000,000+
    AlReader -any text book reader : 5,000,000+
    Offline English Dictionary : 100,000+
    Offline: English to Tagalog Dictionary : 500,000+
    FamilySearch Tree : 1,000,000+
    Cloud of Books : 1,000,000+
    Recipes of Prophetic Medicine for free : 500,000+
    ReadEra – free ebook reader : 1,000,000+
    Anonymous caller detection : 10,000+
    Ebook Reader : 5,000,000+
    Litnet - E-books : 100,000+
    Read books online : 5,000,000+
    English to Urdu Dictionary : 500,000+
    eBoox: book reader fb2 epub zip : 1,000,000+
    English Persian Dictionary : 500,000+
    Flybook : 500,000+
    All Maths Formulas : 1,000,000+
    Ancestry : 5,000,000+
    HTC Help : 10,000,000+
    English translation from Bengali : 100,000+
    Pdf Book Download - Read Pdf Book : 100,000+
    Free Book Reader : 100,000+
    eBoox new: Reader for fb2 epub zip books : 50,000+
    Only 30 days in English, the guideline is guaranteed : 500,000+
    Moon+ Reader : 10,000,000+
    SH-02J Owner's Manual (Android 8.0) : 50,000+
    English-Myanmar Dictionary : 1,000,000+
    Golden Dictionary (EN-AR) : 1,000,000+
    All Language Translator Free : 1,000,000+
    Azpen eReader : 500,000+
    URBANO V 02 instruction manual : 100,000+
    Bible : 100,000,000+
    C Programs and Reference : 50,000+
    C Offline Tutorial : 1,000+
    C Programs Handbook : 50,000+
    Amazon Kindle : 100,000,000+
    Aab e Hayat Full Novel : 100,000+
    Aldiko Book Reader : 10,000,000+
    Google I/O 2018 : 500,000+
    R Language Reference Guide : 10,000+
    Learn R Programming Full : 5,000+
    R Programing Offline Tutorial : 1,000+
    Guide for R Programming : 5+
    Learn R Programming : 10+
    R Quick Reference Big Data : 1,000+
    V Made : 100,000+
    Wattpad 📖 Free Books : 100,000,000+
    Dictionary - WordWeb : 5,000,000+
    Guide (for X-MEN) : 100,000+
    AC Air condition Troubleshoot,Repair,Maintenance : 5,000+
    AE Bulletins : 1,000+
    Ae Allah na Dai (Rasa) : 10,000+
    50000 Free eBooks & Free AudioBooks : 5,000,000+
    Ag PhD Field Guide : 10,000+
    Ag PhD Deficiencies : 10,000+
    Ag PhD Planting Population Calculator : 1,000+
    Ag PhD Soybean Diseases : 1,000+
    Fertilizer Removal By Crop : 50,000+
    A-J Media Vault : 50+
    Al-Quran (Free) : 10,000,000+
    Al Quran (Tafsir & by Word) : 500,000+
    Al Quran Indonesia : 10,000,000+
    Al'Quran Bahasa Indonesia : 10,000,000+
    Al Quran Al karim : 1,000,000+
    Al-Muhaffiz : 50,000+
    Al Quran : EAlim - Translations & MP3 Offline : 5,000,000+
    Al-Quran 30 Juz free copies : 500,000+
    Koran Read &MP3 30 Juz Offline : 1,000,000+
    Hafizi Quran 15 lines per page : 1,000,000+
    Quran for Android : 10,000,000+
    Surah Al-Waqiah : 100,000+
    Hisnul Al Muslim - Hisn Invocations & Adhkaar : 100,000+
    Satellite AR : 1,000,000+
    Audiobooks from Audible : 100,000,000+
    Kinot & Eichah for Tisha B'Av : 10,000+
    AW Tozer Devotionals - Daily : 5,000+
    Tozer Devotional -Series 1 : 1,000+
    The Pursuit of God : 1,000+
    AY Sing : 5,000+
    Ay Hasnain k Nana Milad Naat : 10,000+
    Ay Mohabbat Teri Khatir Novel : 10,000+
    Arizona Statutes, ARS (AZ Law) : 1,000+
    Oxford A-Z of English Usage : 1,000,000+
    BD Fishpedia : 1,000+
    BD All Sim Offer : 10,000+
    Youboox - Livres, BD et magazines : 500,000+
    B&H Kids AR : 10,000+
    B y H Niños ES : 5,000+
    Dictionary.com: Find Definitions for English Words : 10,000,000+
    English Dictionary - Offline : 10,000,000+
    Bible KJV : 5,000,000+
    Borneo Bible, BM Bible : 10,000+
    MOD Black for BM : 100+
    BM Box : 1,000+
    Anime Mod for BM : 100+
    NOOK: Read eBooks & Magazines : 10,000,000+
    NOOK Audiobooks : 500,000+
    NOOK App for NOOK Devices : 500,000+
    Browsery by Barnes & Noble : 5,000+
    bp e-store : 1,000+
    Brilliant Quotes: Life, Love, Family & Motivation : 1,000,000+
    BR Ambedkar Biography & Quotes : 10,000+
    BU Alsace : 100+
    Catholic La Bu Zo Kam : 500+
    Khrifa Hla Bu (Solfa) : 10+
    Kristian Hla Bu : 10,000+
    SA HLA BU : 1,000+
    Learn SAP BW : 500+
    Learn SAP BW on HANA : 500+
    CA Laws 2018 (California Laws and Codes) : 5,000+
    Bootable Methods(USB-CD-DVD) : 10,000+
    cloudLibrary : 100,000+
    SDA Collegiate Quarterly : 500+
    Sabbath School : 100,000+
    Cypress College Library : 100+
    Stats Royale for Clash Royale : 1,000,000+
    GATE 21 years CS Papers(2011-2018 Solved) : 50+
    Learn CT Scan Of Head : 5,000+
    Easy Cv maker 2018 : 10,000+
    How to Write CV : 100,000+
    CW Nuclear : 1,000+
    CY Spray nozzle : 10+
    BibleRead En Cy Zh Yue : 5+
    CZ-Help : 5+
    Modlitební knížka CZ : 500+
    Guide for DB Xenoverse : 10,000+
    Guide for DB Xenoverse 2 : 10,000+
    Guide for IMS DB : 10+
    DC HSEMA : 5,000+
    DC Public Library : 1,000+
    Painting Lulu DC Super Friends : 1,000+
    Dictionary : 10,000,000+
    Fix Error Google Playstore : 1,000+
    D. H. Lawrence Poems FREE : 1,000+
    Bilingual Dictionary Audio App : 5,000+
    DM Screen : 10,000+
    wikiHow: how to do anything : 1,000,000+
    Dr. Doug's Tips : 1,000+
    Bible du Semeur-BDS (French) : 50,000+
    La citadelle du musulman : 50,000+
    DV 2019 Entry Guide : 10,000+
    DV 2019 - EDV Photo & Form : 50,000+
    DV 2018 Winners Guide : 1,000+
    EB Annual Meetings : 1,000+
    EC - AP & Telangana : 5,000+
    TN Patta Citta & EC : 10,000+
    AP Stamps and Registration : 10,000+
    CompactiMa EC pH Calibration : 100+
    EGW Writings 2 : 100,000+
    EGW Writings : 1,000,000+
    Bible with EGW Comments : 100,000+
    My Little Pony AR Guide : 1,000,000+
    SDA Sabbath School Quarterly : 500,000+
    Duaa Ek Ibaadat : 5,000+
    Spanish English Translator : 10,000,000+
    Dictionary - Merriam-Webster : 10,000,000+
    JW Library : 10,000,000+
    Oxford Dictionary of English : Free : 10,000,000+
    English Hindi Dictionary : 10,000,000+
    English to Hindi Dictionary : 5,000,000+
    EP Research Service : 1,000+
    Hymnes et Louanges : 100,000+
    EU Charter : 1,000+
    EU Data Protection : 1,000+
    EU IP Codes : 100+
    EW PDF : 5+
    BakaReader EX : 100,000+
    EZ Quran : 50,000+
    FA Part 1 & 2 Past Papers Solved Free – Offline : 5,000+
    La Fe de Jesus : 1,000+
    La Fe de Jesús : 500+
    Le Fe de Jesus : 500+
    Florida - Pocket Brainbook : 1,000+
    Florida Statutes (FL Code) : 1,000+
    English To Shona Dictionary : 10,000+
    Greek Bible FP (Audio) : 1,000+
    Golden Dictionary (FR-AR) : 500,000+
    Fanfic-FR : 5,000+
    Bulgarian French Dictionary Fr : 10,000+
    Chemin (fr) : 1,000+
    The SCP Foundation DB fr nn5n : 1,000+


There are only a few very popular apps in this category:


```python
for app in android_free:
    if app[1] == 'BOOKS_AND_REFERENCE' and (app[5] == '1,000,000,000+'
                                            or app[5] == '500,000,000+'
                                            or app[5] == '100,000,000+'):
        print(app[0], ':', app[5])
```

    Google Play Books : 1,000,000,000+
    Bible : 100,000,000+
    Amazon Kindle : 100,000,000+
    Wattpad 📖 Free Books : 100,000,000+
    Audiobooks from Audible : 100,000,000+


Let's try to get some app ideas based on the kind of apps that are somewhere in the middle in terms of popularity (between 1,000,000 and 100,000,000 downloads):


```python
for app in android_free:
    if app[1] == 'BOOKS_AND_REFERENCE' and (app[5] == '1,000,000+'
                                            or app[5] == '5,000,000+'
                                            or app[5] == '10,000,000+'
                                            or app[5] == '50,000,000+'):
        print(app[0], ':', app[5])
```

    Wikipedia : 10,000,000+
    Cool Reader : 10,000,000+
    Book store : 1,000,000+
    FBReader: Favorite Book Reader : 10,000,000+
    Free Books - Spirit Fanfiction and Stories : 1,000,000+
    AlReader -any text book reader : 5,000,000+
    FamilySearch Tree : 1,000,000+
    Cloud of Books : 1,000,000+
    ReadEra – free ebook reader : 1,000,000+
    Ebook Reader : 5,000,000+
    Read books online : 5,000,000+
    eBoox: book reader fb2 epub zip : 1,000,000+
    All Maths Formulas : 1,000,000+
    Ancestry : 5,000,000+
    HTC Help : 10,000,000+
    Moon+ Reader : 10,000,000+
    English-Myanmar Dictionary : 1,000,000+
    Golden Dictionary (EN-AR) : 1,000,000+
    All Language Translator Free : 1,000,000+
    Aldiko Book Reader : 10,000,000+
    Dictionary - WordWeb : 5,000,000+
    50000 Free eBooks & Free AudioBooks : 5,000,000+
    Al-Quran (Free) : 10,000,000+
    Al Quran Indonesia : 10,000,000+
    Al'Quran Bahasa Indonesia : 10,000,000+
    Al Quran Al karim : 1,000,000+
    Al Quran : EAlim - Translations & MP3 Offline : 5,000,000+
    Koran Read &MP3 30 Juz Offline : 1,000,000+
    Hafizi Quran 15 lines per page : 1,000,000+
    Quran for Android : 10,000,000+
    Satellite AR : 1,000,000+
    Oxford A-Z of English Usage : 1,000,000+
    Dictionary.com: Find Definitions for English Words : 10,000,000+
    English Dictionary - Offline : 10,000,000+
    Bible KJV : 5,000,000+
    NOOK: Read eBooks & Magazines : 10,000,000+
    Brilliant Quotes: Life, Love, Family & Motivation : 1,000,000+
    Stats Royale for Clash Royale : 1,000,000+
    Dictionary : 10,000,000+
    wikiHow: how to do anything : 1,000,000+
    EGW Writings : 1,000,000+
    My Little Pony AR Guide : 1,000,000+
    Spanish English Translator : 10,000,000+
    Dictionary - Merriam-Webster : 10,000,000+
    JW Library : 10,000,000+
    Oxford Dictionary of English : Free : 10,000,000+
    English Hindi Dictionary : 10,000,000+
    English to Hindi Dictionary : 5,000,000+


This category of apps seems to be dominated by apps for processing and reading e-books, libraries, dictionaries which are very similar to each other in terms of functionality. There are also many apps built around religious books which could imply that providing a supportive resource of a popular book can be a way of creating a demand in market. 

We also notice there are quite a few apps built around the book Quran, which suggests that building an app around a popular book can be profitable. It seems that taking a popular book (perhaps a more recent book) and turning it into an app could be profitable for both the Google Play and the App Store markets. The market is full of free or paid libraries, which makes it hard to compete with only providing a raw book to users so what is pushed to market might provide some additional value. For instance, instead of the raw book, providing quotes, curation of ideas and concepts, a platform of sharing ideas around a certain book/writer/genre would give you an alternative road to follow, instead of competing with apps that have been downloaded millions of times already.

## To Conclude...

In this very first project in dataquest, I've analyzed mobile app data from App Store and Google Store to recommend an app profile that can be profitable for both markets. Obviously, what we've found from this simple analysis is very superficial but at the same time, it shows how combination of simple python skills and data sets that are available for free can lead you maybe not a conclusion, but an idea and general overview of what you are working on. It seems like in most popular categories, there are giant apps that are very hard to compete. On the other hand, in the books and references categories, though there are very few giants, there is still hope for an app which challenges the other apps having the raw version of a popular book with providing a discussion platform, curation of supportive resources, audio versions, etc. 

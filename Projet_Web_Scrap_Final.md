---
title: Projet WebScraping
image: pic01.jpg
---
# <center> <font face="Georgia, serif" color="#000" size=12px>Projet de WebScraping</font></center>

<font face="Lucida Sans Unicode, Lucida Grande, sans-serif" size="3px">J'aime lire. Des livres,de la presse, des magazines. J'adore faire de la veille et j'apprécie par dessus tout partager mes trouvailles, faire de la <b>curation</b>. Grâce au web, on peut aujourd'hui accéder à des journaux, magazines etc... du monde entier qu'il était difficile de trouver en version papier.<br/>

Pour ce projet de Webscraping, J'ai choisi de scraper 2 sites de médias web : [**Lily**](https://www.thelily.com "Title") et [**Nylon**](https://nylon.com).<br/><br/>
<table>
    <tr>
        <td> <img src="lily.png" alt="Lily_logo" style="width: 100px;"/> </td>
        <td>  </td>
        <td> <img src="nylon_BIS.jpg" alt="nylon_logo" style="width: 100px;"/> </td>
    </tr>
</table><br/>
<b>Objectif</b> : Créer un tableau qui, pour un mot donnée, rassemble les articles issus des pages de résultats de recherche de ces deux sites.<br/>
<b>Le challenge</b> : réussir à uniformiser les résultats afin d'obtenir un tableau final contenant toutes les données.</font>

### Import des librairies


```python
import pandas as pd
from bs4 import BeautifulSoup
import requests
import re
```

### Création d'un champs input pour entrer le mot à chercher de son choix


```python
word = input("Tapez un mot : ")

url_lily = f'https://www.thelily.com/tag/{word}/'
url_nylon = f'https://nylon.com/search/?q={word}'

html_lily = BeautifulSoup(requests.get(url_lily).content)
nylon_html=BeautifulSoup(requests.get(url_nylon).content)
```

    Tapez un mot : psychology


### Travail sur les données issues de  Lily News
Mise en forme des données scrappées


```python
lily_card=html_lily.find_all('div', class_='card-content align-items-start flex-container-row justify-space-between flex-mobile-column')

lily_article =[]
for x in lily_card:
    a=x.find_all('a')
    h3 = x.find_all('h3')
    lily_article.append([a,h3])
```


```python
df_lily = pd.DataFrame(lily_article)
df_lily.columns=['titre','auteur']
df_lily['theme']=df_lily['titre']
```


```python
def cleandf(a):
    return '|'.join([x.text.strip() for x in a])

dfcol=['titre','auteur']

for col in dfcol:
    df_lily[col]=list(map(cleandf, df_lily[col]))
```


```python
def linkdf(b):   
    return '|'.join([x.get('href') for x in b])

df_lily['theme']=list(map(linkdf, df_lily['theme']))
```


```python
link = lambda a:'https://www.thelily.com'+str(a)
df_lily['theme']=list(map(link,df_lily['theme']))
```


```python
df_lily['link'] = df_lily['theme'].str.split('|').str[0]
df_lily['theme'] = df_lily['titre'].str.split('|').str[1]
df_lily['titre'] = df_lily['titre'].str.split('|').str[2]
df_lily['accroche'] = df_lily['auteur'].str.split('|').str[0]
df_lily['date'] = df_lily['auteur'].str.split('|').str[2]
df_lily['auteur'] = df_lily['auteur'].str.split('|').str[1]
df_lily['source'] = 'The Lily'
```


```python
df_lily = df_lily[['source', 'theme','titre','auteur','accroche','date','link',]]
df_lily.head(3)
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
      <th>source</th>
      <th>theme</th>
      <th>titre</th>
      <th>auteur</th>
      <th>accroche</th>
      <th>date</th>
      <th>link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Lily</td>
      <td>Books</td>
      <td>I’m a therapist, and a shocking breakup landed...</td>
      <td>Lori Gottlieb</td>
      <td>An excerpt from Lori Gottlieb’s ‘Maybe You Sho...</td>
      <td>May 3</td>
      <td>https://www.thelily.com/im-a-therapist-and-a-s...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Lily</td>
      <td>Family</td>
      <td>Two decades after disappearing, her daughter s...</td>
      <td>The Lily News</td>
      <td>She returned with children and a new name, and...</td>
      <td>March 14</td>
      <td>https://www.thelily.com/two-decades-after-disa...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Lily</td>
      <td>Books</td>
      <td>This book’s heroine unexpectedly sees her abus...</td>
      <td>Maureen Corrigan</td>
      <td>What comes next in Christobel Kent’s psycholog...</td>
      <td>February 8</td>
      <td>https://www.thelily.com/this-books-heroine-une...</td>
    </tr>
  </tbody>
</table>
</div>



### Travail sur les données issues de Nylon


```python
nylon_card=nylon_html.find_all('div', class_='widget')
card = [x.find_all('article') for x in nylon_card]

nylon_article=[y.find_all(['a','span'])for x in card for y in x]

df_nylon = pd.DataFrame(nylon_article)
df_nylon = df_nylon.rename(columns={0: 'theme',1:'link',2:'titre',3:'auteur',4:'date'})
df_nylon['source']='nylon'
df_nylon['accroche']=''
```


```python
dfcol=['theme','titre','auteur','date']
for col in dfcol:
    df_nylon[col]=([x.text.strip() for x in df_nylon[col]])

df_nylon['link'] = [x.get('href') for x in df_nylon['link']]
df_nylon = df_nylon[['source','theme','titre','auteur','accroche','date','link']]
```


```python
df_nylon.head(3)
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
      <th>source</th>
      <th>theme</th>
      <th>titre</th>
      <th>auteur</th>
      <th>accroche</th>
      <th>date</th>
      <th>link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>nylon</td>
      <td>Film</td>
      <td>Megan Fox Says She Had A "Psychological Breakd...</td>
      <td>Bailey Calfee</td>
      <td></td>
      <td>19 September</td>
      <td>https://nylon.com/megan-fox-psychological-brea...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>nylon</td>
      <td>Justice</td>
      <td>The Psychological Impact Of Anti-Abortion Legi...</td>
      <td>Ivana Rihter</td>
      <td></td>
      <td>21 August</td>
      <td>https://nylon.com/psychological-impact-anti-ab...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>nylon</td>
      <td>TV</td>
      <td>Alia Shawkat Can't Wait To Lose Control</td>
      <td>Caitlin Wolper</td>
      <td></td>
      <td>16 October</td>
      <td>https://nylon.com/alia-shawkat-second-woman-in...</td>
    </tr>
  </tbody>
</table>
</div>



### Fusion des deux tableaux


```python
df_search_word = df_lily.append(df_nylon)
df_search_word
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
      <th>source</th>
      <th>theme</th>
      <th>titre</th>
      <th>auteur</th>
      <th>accroche</th>
      <th>date</th>
      <th>link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Lily</td>
      <td>Books</td>
      <td>I’m a therapist, and a shocking breakup landed...</td>
      <td>Lori Gottlieb</td>
      <td>An excerpt from Lori Gottlieb’s ‘Maybe You Sho...</td>
      <td>May 3</td>
      <td>https://www.thelily.com/im-a-therapist-and-a-s...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Lily</td>
      <td>Family</td>
      <td>Two decades after disappearing, her daughter s...</td>
      <td>The Lily News</td>
      <td>She returned with children and a new name, and...</td>
      <td>March 14</td>
      <td>https://www.thelily.com/two-decades-after-disa...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>The Lily</td>
      <td>Books</td>
      <td>This book’s heroine unexpectedly sees her abus...</td>
      <td>Maureen Corrigan</td>
      <td>What comes next in Christobel Kent’s psycholog...</td>
      <td>February 8</td>
      <td>https://www.thelily.com/this-books-heroine-une...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>The Lily</td>
      <td>Lifestyle</td>
      <td>Minimalism isn’t for me but here’s how I’m get...</td>
      <td>Monica Castillo</td>
      <td>After years of constant moving, I’m finally or...</td>
      <td>July 16</td>
      <td>https://www.thelily.com/minimalism-isnt-for-me...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The Lily</td>
      <td>Violence</td>
      <td>The Annapolis shooting was an attack on a news...</td>
      <td>Elizabeth Chang</td>
      <td>The fight isn’t just about guns or the news media</td>
      <td>July 2</td>
      <td>https://www.thelily.com/the-annapolis-shooting...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>The Lily</td>
      <td>Obituaries</td>
      <td>Psychologist Anne Treisman ‘changed the way we...</td>
      <td>The Lily News</td>
      <td>She died last week at 82</td>
      <td>February 15</td>
      <td>https://www.thelily.com/psychologist-anne-trei...</td>
    </tr>
    <tr>
      <th>0</th>
      <td>nylon</td>
      <td>Film</td>
      <td>Megan Fox Says She Had A "Psychological Breakd...</td>
      <td>Bailey Calfee</td>
      <td></td>
      <td>19 September</td>
      <td>https://nylon.com/megan-fox-psychological-brea...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>nylon</td>
      <td>Justice</td>
      <td>The Psychological Impact Of Anti-Abortion Legi...</td>
      <td>Ivana Rihter</td>
      <td></td>
      <td>21 August</td>
      <td>https://nylon.com/psychological-impact-anti-ab...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>nylon</td>
      <td>TV</td>
      <td>Alia Shawkat Can't Wait To Lose Control</td>
      <td>Caitlin Wolper</td>
      <td></td>
      <td>16 October</td>
      <td>https://nylon.com/alia-shawkat-second-woman-in...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>nylon</td>
      <td>Music</td>
      <td>Artist To Watch Mahalia Hates Drama, But Not T...</td>
      <td>Allison Stubblebine</td>
      <td></td>
      <td>29 October</td>
      <td>https://nylon.com/mahalia-love-compromise-inte...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>nylon</td>
      <td>Film</td>
      <td>Everyone Who Wanted To See Natalie Portman In ...</td>
      <td>Allison Stubblebine</td>
      <td></td>
      <td>12 September</td>
      <td>https://nylon.com/lucy-sky-natalie-portman-dia...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>nylon</td>
      <td>Film</td>
      <td>'Don't Let Go' Is Not Just Another Time Travel...</td>
      <td>Sesali Bowen</td>
      <td></td>
      <td>28 August</td>
      <td>https://nylon.com/storm-reid-david-oyelowo-int...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>nylon</td>
      <td>Film</td>
      <td>In 'The Lighthouse' Robert Pattinson Descends ...</td>
      <td>Jesse Hassenger</td>
      <td></td>
      <td>17 October</td>
      <td>https://nylon.com/the-lighthouse-review-robert...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>nylon</td>
      <td>Astrology</td>
      <td>Here Are The Zodiac Signs For All The Characte...</td>
      <td>Gala Mukomolova</td>
      <td></td>
      <td>14 October</td>
      <td>https://nylon.com/zodiac-signs-euphoria-charac...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>nylon</td>
      <td>Justice</td>
      <td>How Art Inspired By Sexual Assault Can Help Su...</td>
      <td>Caitlin Wolper</td>
      <td></td>
      <td>19 November</td>
      <td>https://nylon.com/sexual-assault-art-healing</td>
    </tr>
    <tr>
      <th>9</th>
      <td>nylon</td>
      <td>Justice</td>
      <td>'The Good Place' Star Jameela Jamil Says Abort...</td>
      <td>Allison Stubblebine</td>
      <td></td>
      <td>14 May</td>
      <td>https://nylon.com/jameela-jamil-abortion</td>
    </tr>
    <tr>
      <th>10</th>
      <td>nylon</td>
      <td>Film</td>
      <td>How Octavia Spencer Became One Of Hollywood's ...</td>
      <td>Jesse Hassenger</td>
      <td></td>
      <td>01 August</td>
      <td>https://nylon.com/octavia-spencer-luce-review</td>
    </tr>
    <tr>
      <th>11</th>
      <td>nylon</td>
      <td>Books</td>
      <td>Jia Tolentino Tells The Stories Of Generation ...</td>
      <td>Joanna Demkiewicz</td>
      <td></td>
      <td>06 August</td>
      <td>https://nylon.com/review-jia-tolentino-trick-m...</td>
    </tr>
  </tbody>
</table>
</div>



### Export dans un fichier .json


```python
df_search_word.to_json('search_word_result.json', orient='records')
```

### Piste d'amélioration et d'avancement

* ajouter d'autres sites, de plusieurs langues
* ajouter un module de traduction pour que la recherche soit possible sur ces différents sites
* compter quand il sont présents le nombre d'articles par thème
* analyser le rythme de publication autour du mots choisis par exemple pour voir la saisonnalité, si une tendance ressort etc...


```python

```

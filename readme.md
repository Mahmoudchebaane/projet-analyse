# PROJET NETFLIX
<img src="/img/logo-epi.png" >

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Mahmoudchebaane/projet-analyse/master)

<a href="https://colab.research.google.com/github/Mahmoudchebaane/projet-analyse/blob/main/Index.ipynb\" target="_parent\"><img src="https://colab.research.google.com/assets/colab-badge.svg\" alt="Open In Colab\"></a>

## DATASET :file_folder: 
Le dataset contient
les donnes suivantes: 
|    | type    | title                | director        | cast                                                                                                                                                                                                                                                                                                              | country       | date_added           |   release_year | rating   | duration   | listed_in                                         | description                                                                                                                                                |
|---:|:--------|:---------------------|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------|:---------------------|---------------:|:---------|:-----------|:--------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|  0 | Movie   | Dick Johnson Is Dead | Kirsten Johnson | nan                                                                                                                                                                                                                                                                                                               | United States | "September 25, 2021" |           2020 | PG-13    | 90 min     | Documentaries                                     | "As her father nears the end of his life, filmmaker Kirsten Johnson stages his death in inventive and comical ways to help them both face the inevitable." |
|  1 | TV Show | Blood & Water        | nan             | "Ama Qamata, Khosi Ngema, Gail Mabalane, Thabang Molaba, Dillon Windvogel, Natasha Thahane, Arno Greeff, Xolile Tshabalala, Getmore Sithole, Cindy Mahlangu, Ryle De Morny, Greteli Fincham, Sello Maake Ka-Ncube, Odwa Gwanya, Mekaila Mathys, Sandi Schultz, Duane Williams, Shamilla Miller, Patrick Mofokeng" | South Africa  | "September 24, 2021" |           2021 | TV-MA    | 2 Seasons  | "International TV Shows, TV Dramas, TV Mysteries" | "After crossing paths at a party, a Cape Town teen sets out to prove whether a private-school swimming star is her sister who was abducted at birth."

## Contexte 
Ce jeu de données contient plus de 8 500 films et séries télévisées Netflix, comprenant les membres du casting, la durée et le genre. Il inclut des titres ajoutés aussi récemment que fin septembre 2021.

<p align ="center"><img src="/img/Netflix.png"  width="150" height="150"> </p>

### Chargement des données
 Les données sont dans un tableau au format CSV (Comma Separeted Values)
 ```python 
 import pandas as pd
 df=pd.read_csv("data/data.csv")
 ```
 ```python 
 df.shape
 # (1818 rows x 11 columns)
 ```
 ```python
 df.describe()
 ```
 ```python
df.info()
 ```
  ```python
df.columns
 ```
 ### Compréhension des variables :
 - type : type de données (Movie / TV Show)
 - title : titre de données
 - director : réalisateur du film
 - cast :  acteurs et actrices 
 - country : pays où la production a été réalisée ou produit
 - date_added : la date à laquelle été ajouté à la bibliothèque de Netflix
 - release_year : année de sortie
 - rating : classification
 - duration : durée
 - listed_in : classification (International TV, Crime TV Shows, documentaire,etc)
 - description : discription de données

## Question / Objectif
 - Quelle est la variété de l'offre de Netflix ? Basez-vous sur trois variables : le type, le pays et les catégories listées.
 - Visualisez : Créez un nuage de mots à partir des descriptions des films et des émissions de télévision. Assurez-vous de supprimer les mots vides (stop words) !
 - Analyser : Est-ce que Netflix a investi davantage dans certains genres (voir les catégories listées) ces dernières années ? Et qu'en est-il de certains groupes d'âge (voir les classifications) ?

 ### Exploration des données
 Voici quelques exemples d'explorations possibles :
 - Exploration de la variété par type :
```python
import pandas as pd
import matplotlib.pyplot as plt
variety_by_type = df['type'].value_counts().head(10)
plt.figure(figsize=(8, 6))
plt.bar(variety_by_type.index, variety_by_type.values)
plt.title('Variety by Type')
plt.xlabel('Type')
plt.ylabel('Count')
plt.show()
```
![Alt text](/img/image-3.png)
- Exploration de la variété par catégorie de contenu 
```python
variety_by_listed_in = df['listed_in'].value_counts().head(10)
plt.figure(figsize=(12, 6))
variety_by_listed_in.plot(kind='bar')
plt.title('Variety by Listed_in (Top 10)')
plt.xlabel('Listed_in')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()
```
  ![Alt text](/img/image-1.png)
 - Exploration de la variété par pays :
```python
variety_by_country = df['country'].value_counts().head(10)
plt.figure(figsize=(12, 6))
variety_by_country.plot(kind='bar')
plt.title('Variety by Country (Top 10)')
plt.xlabel('Country')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show() 
```
![Alt text](/img/image-2.png)

### Visualisation
- Nuage de mots apartir de description : 
```python
import pandas as pd
import matplotlib.pyplot as plt
from wordcloud import WordCloud, STOPWORDS

# Concaténez les descriptions en une seule chaîne de caractères
descriptions = " ".join(df['description'].dropna())

# Remove stopwords
stop_words = set(stopwords.words('english'))
filtered_descriptions = ' '.join([word for word in descriptions.split() if word.lower() not in stop_words])

# Créez un objet WordCloud en spécifiant les paramètres souhaités
wordcloud = WordCloud(stopwords=stop_words, background_color='white').generate(filtered_descriptions)

# Display the word cloud
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.show()
```
![Alt text](/img/image-4.png)

### Analyse si Netflix a investi davantage dans certains genres et tranches d'âge ces dernières années
  ```python
  # Extrayez les colonnes pertinentes pour l'analyse
  subset = df[['release_year', 'listed_in', 'rating']]
 
  # Filtrez l'ensemble de données pour les années récentes
  current_year = pd.Timestamp.now().year
  recent_subset = subset[subset['release_year'] >= current_year - 5]

 # Analysez l'investissement dans le genre :
 genre_counts = recent_subset['listed_in'].value_counts().head(10)
plt.figure(figsize=(10, 6))
genre_counts.plot(kind='bar')
plt.title('Genre Investment in Recent Years')
plt.xlabel('Genre')
plt.ylabel('Count')
plt.show()
```
![Alt text](/img/image-5.png)

```python
# Analyser l'investissement par tranche d'âge
age_counts = recent_subset['rating'].value_counts().sort_index()
plt.figure(figsize=(10, 6))
age_counts.plot(kind='bar')
plt.title('Age Group Investment in Recent Years')
plt.xlabel('Age Group')
plt.ylabel('Count')
plt.show()
```
![Alt text](/img/image-6.png)

### Distribution cumulative des movie sur Netflix
```python
import pandas as pd
import matplotlib.pyplot as plt
# Filtration 
movies = df[df['type'] == 'Movie']
# Triez les films par année de sortie
movies = movies.sort_values('release_year')
#Créez une distribution cumulée
cumulative_counts = range(1, len(movies) + 1)
cumulative_percentages = [count / len(movies) * 100 for count in cumulative_counts]
# Tracer la distribution cumulative
plt.figure(figsize=(10, 6))
plt.plot(movies['release_year'], cumulative_percentages)
plt.title('Cumulative Distribution of Movies on Netflix')
plt.xlabel('Release Year')
plt.ylabel('Cumulative Percentage')
plt.xticks(rotation=45)
plt.show()
```
![Alt text](/img/image-7.png)

### Distribution cumulative des TV Shows sur Netflix par années
```python
# Filtration 
tv_shows = df[df['type'] == 'TV Show']

# Triez les tv show par année
grouped_tv_shows = tv_shows.groupby('release_year').size().sort_values(ascending=False)
# Traçage
plt.figure(figsize=(10, 6))
grouped_tv_shows.plot(kind='bar')
plt.title('Distribution of TV Shows on Netflix')
plt.xlabel('Genre')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()
```
![Alt text](/img/image-8.png)


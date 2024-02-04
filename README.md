# The Movie Database(TMDb) data extraction using API calling
<p>This project is all about how to use API to extract data. An API, which stands for Application Programming Interface, is a set of defined rules that enable different applications to communicate with each other. It acts as an intermediary layer that processes data transfers between systems, allowing companies to open their application data and functionality to external third-party developers, business partners, and internal departments within their organizations. For example: OpenAI, reddit, google provide API to use their codes and data in order to create new applications or software. 
</p>
<p>Similary, TMDb provides API to access its full data about movies, TV shows, actors and  many other demography. So, I used TMDb API and extracts several data from the database, below you can see how and what data I have extracted.</p>
<h2>How to call API</h2>
<p>Every applicatin has their own steps and rules to request their APIs. For TMDb, you need to log in to their website and generate an API key. After this create an application. Now use this API key or access token in open authentication and access the data</p>
<p>Follow this link to get more clarification and step by step guide: https://developer.themoviedb.org/docs/getting-started</p>
<h2>Tools Used</h2>
<ul>
  <li>Python and Requests(python library)</li>
</ul>
<h3>Find the 'id' of the movie "Andhadhun" using TMDb API</h3>

```python
import requests
api_key = "e226f4a5f5bace766952aa0d17182959"
api_link = "https://api.themoviedb.org/3"
params = {'query':"Andhadhun", 'api_key':api_key}
header = {'Accept': 'application/json'}
response = requests.get(api_link + "/search/movie", headers= header, params=params)
data = response.json()
results = data.get('results')
for result in results:
    if result.get('title') == 'Andhadhun':
        print(result.get('id'))
```
<h3>Fetch the company id of company 'Marvel Studios' </h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}

r = requests.get("https://api.themoviedb.org/3/search/company", params = {"query":"Marvel Studios"}, headers = headers)
r.status_code
data = r.json()
for i in data['results']:
    if i['name'] == "Marvel Studios":
        print(i['id'])
```
<h3>Find the vote count and vote average of the movie "3 Idiots"</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}

r = requests.get("https://api.themoviedb.org/3/search/movie", params = {"query":"3 Idiots"}, headers = headers)
r.status_code
data = r.json()
for i in data['results']:
    if i['original_title'] == "3 Idiots":
        print(i['vote_count'],i['vote_average'])
```
<h3>Fetch the names of top 5 similar movies to 'Inception'</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}

url = "https://api.themoviedb.org/3/movie/27205/similar"
r = requests.get(url, headers = headers)
data = r.json()
similarmovie=(movie['title'] for movie in data['results'][:5])
for title in similarmovie:
    print(title)
```
<h3>Fetch the top rated english movies in the US region print the first 10 movies which have original language as english.</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}


url= "https://api.themoviedb.org/3/genre/movie/list"
r = requests.get(url, params = {"language":"en"}, headers = headers)
data = r.json()
genre = data.get("genres")
genres = {}

for i in genre:
    genres[i['id']] = i['name']
    
url = 'https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=1&sort_by=vote_average.desc&without_genres=99,10755&vote_count.gte=200'
res = requests.get(url, params = {"language":"en-US"}, headers = headers)
data = res.json()
y = data['results']
title = []
genre_ID = []
for i in y:
    if i['original_language'] == 'en':
        title.append(i['original_title'])
        genre_ID.append(i['genre_ids'])
        
for j in range(10):
    print(title[j], '-', end=" ")
    for k in genre_ID[j]:
        print(genres[k], end=', ')
    print()
```
<h3>Find the name and birthplace of the present most popular person according to TMDb API.</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}

url = "https://api.themoviedb.org/3/person/popular"
r = requests.get(url, headers = headers)

data = r.json()
person = data['results'][0]
print(person['id'])

url2 = "https://api.themoviedb.org/3/person/1290466"
r1 = requests.get(url2, headers = headers)

data1 = r1.json()
print(data1['name'],"-",data1['place_of_birth'])
```
<h3>Fetch the Instagram and Twitter handle of Indian Actress "Alia Bhatt" from the TMDb API.</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}


url = "https://api.themoviedb.org/3/search/person"
r = requests.get(url, params = {"query":"Alia Bhatt"}, headers = headers)  #headers contain API key, refer 1st code block
data = r.json()
person = data['results'][0]
#print(person['id'])

url2 = "https://api.themoviedb.org/3/person/1108120/external_ids"    #put person_id in the url exctracted from 1st URL
res = requests.get(url2, headers = headers)

info = res.json()
print(info['instagram_id'],"",info["twitter_id"])
```
<h3>Fetch the names of the character played by Tom Cruise in the movies:
<ul>
<li>Top Gun</li>
<li>Mission: Impossible - Fallout</li>
<li>Minority Report</li>
<li>Edge of Tomorrow</li>
</ul>
</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}

url = "https://api.themoviedb.org/3/search/person"
r = requests.get(url, params = {"query":"Tom Cruise"}, headers = headers)  #headers contain API key, refer 1st code block
data = r.json()
person = data['results'][0]
#print(person['id'])

url2 = "https://api.themoviedb.org/3/person/500/movie_credits"
res = requests.get(url2, headers = headers)

info = res.json()
movies = ["Top Gun", "Mission: Impossible - Fallout", "Minority Report", "Edge of Tomorrow"]
for i in movies:
    for j in info['cast']:
        if j['title'] == i:
            print(j['character'])
            break
```
<h3>Did James McAvoy play a role in the movie Deadpool 2</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}

r = requests.get("https://api.themoviedb.org/3/search/movie", params = {"query":"Deadpool 2"}, headers = headers)
data = r.json()
y = data['results'][1]
#print(y['id'])

res = requests.get("https://api.themoviedb.org/3/movie/567604/credits", headers = headers)

info = res.json()

for i in info['cast']:
    if i['name'] == "James McAvoy":
        print("yes")
```
<h3>Fetch the overview of the TV Show "FRIENDS" </h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}
api_link = "https://api.themoviedb.org/3"

response2 = requests.get("https://api.themoviedb.org/3/search/tv",params = {"query":"Friends"}, headers = headers)
data=response2.json()

for i in data['results']:
    if i['original_name'] == "Friends":
        print(i['overview'])
```
<h3>Fetch the name and air date of S06E05 of the TV Show 'The Big Bang Theory'</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}
base_url = 'https://api.themoviedb.org/3'

tv_show_id = 1418
season_number = 6
episode_number = 5
url = f'{base_url}/tv/{tv_show_id}/season/{season_number}/episode/{episode_number}'

response = requests.get(url, headers = headers)
data = response.json()

name = data['name']
air_date = data['air_date']
print(name," - ",air_date)
```
<h3>Count the number of males and females in the cast of "Money Heist"</h3>

```python
import requests
headers = {
    "accept": "application/json",
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI5OTlhNDI1NGIyNTg4YjY1MTdlZThmMDMwY2RiZWJmOCIsInN1YiI6IjY1OGYxYWE2NjM1MzZhNWQxNGI4NjBkMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.ZjeViv4_i83uaGVPMIQK5OR3l5uIM8i0uFQFlW7jp2A"
}

r = requests.get("https://api.themoviedb.org/3/tv/71446/credits", headers = headers)
r.status_code
data = r.json()
male = 0
female = 0
for i in data['cast']:
    if i['gender'] == 1:
        female = female + 1
    if i['gender'] == 2:
        male = male + 1
        
print(male, female)
```

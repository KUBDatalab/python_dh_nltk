---
title: "Python, Digital Humaniora og NLTK"
teaching: 10
exercises: 0
questions:
- "Dyk ned i NTLK"
objectives:
- "Loops"
- "Lists"
- "Stings"

keypoints:
- "Kendskab til NLTK"
---

# Import biblioteker!!

```python
import urllib.request
import re
import nltk
```


```python
# hent teksten
url = 'https://www.gutenberg.org/files/2591/2591-0.txt'
raw_text = urllib.request.urlopen(url).read().decode()
```


```python
#find starten
start = raw_text.find('THE BROTHERS GRIMM FAIRY TALES') + len('THE BROTHERS GRIMM FAIRY TALES')
# find slutningen
end = raw_text.find('*****')
# find det der imellem
content = raw_text[start:end]

#opdel på hvert eventyr 
tales = content.split('\r\n\r\n\r\n\r\n\r\n') # split før overskrift på titel
# Filtrer første item væk, fordi det er tomt
tales = tales[1:] 

titles = []
for i in tales:
    split_text = i.split('\r\n\r\n\r\n') # split efter titel
    title = split_text[0] # gem første del af splittet - det er titlen  
    titles.append(title) # tilføj titlen til den tomme liste

# zip titles og tales    
zip_format = list(zip(titles, tales))
```


# Os biblioteket


```python
import os
```


```python
os.getcwd()
```

```Out


    'your own path'

```

```python
# lav en ny mappe
# os.mkdir('grimm_tales')
os.chdir('grimm_tales')
```


```python
# Gem filerne i mappen
for i in zip_format:
    new_file = open(i[0]+'.txt', 'w', encoding='utf-8-sig')
    new_file.write(i[1])
    new_file.close()

# check om de er der der!    
os.listdir()
```

```Out


    ['.ipynb_checkpoints',
     'ASHPUTTEL.txt',
     'BRIAR ROSE.txt',
     'CAT AND MOUSE IN PARTNERSHIP.txt',
     'CAT-SKIN.txt',
     'CLEVER ELSIE.txt',
     'CLEVER GRETEL.txt'
	 
	 '...']

```

# NLTK metoder


```python
import nltk
text = tales[0]
lower_text = text.lower()
scrub_text = ' '.join(re.findall(r'\S+', lower_text))
tokenized_text = nltk.word_tokenize(scrub_text)
nltk_text = nltk.Text(tokenized_text)
stopwords = nltk.corpus.stopwords.words('english')
newStopWords = [',',';', '.', ':', '‘', '’']
stopwords.extend(newStopWords)
filtered_tokens = [word for word in nltk_text if word not in stopwords]
filtered_tokens
```
```Out



    ['golden',
     'bird',
     'certain',
     'king',
     'beautiful',
     'garden',
     'garden'
     ...]
```



```python
fdist_filtered = nltk.FreqDist(filtered_tokens).plot(20, title='Hyppigste ord (uden stopord)')
```


    
![png](fig/output_13_0.png)
    


Collocation_list() returnerer en liste over de mest almindelige ordpar i teksten. Bemærk, at i nogle versioner af Python virker collocation_list() ikke. Hvis dette er tilfældet, prøv collocations() i stedet.


```python
nltk_text.collocation_list()
```

```Out

    [('young', 'man'),
     ('golden', 'bird'),
     ('hair', 'whistled'),
     ('two', 'inns'),
     ('stone', 'till'),
     ('take', 'leave'),
     ('good', 'counsel'),
     ('time', 'passed'),
     ('eldest', 'son'),
     ('leathern', 'saddle'),
     ('fox', 'came'),
     ('morning', 'another'),
     ('wooden', 'cage'),
     ('two', 'men'),
     ('fast', 'asleep'),
     ('old', 'fox'),
     ('fell', 'asleep'),
     ('fox', 'said'),
     ('second', 'son'),
     ('youngest', 'son')]

```

Concordance()-metoden returnerer konteksten af et specifikt udtryk. Længden af output kan ændres med parametrene i width og lines.


```python
nltk_text.concordance('golden', lines=30, width=80)
```

    Displaying 22 of 22 matches:
    the golden bird a certain king had a beautiful 
    n the garden stood a tree which bore golden apples . these apples were always co
    the bird no harm ; only it dropped a golden feather from its tail , and then fle
     its tail , and then flew away . the golden feather was brought to the king in t
     son set out and thought to find the golden bird very easily ; and when he had g
    s is , and that you want to find the golden bird . you will reach a village in t
    ation , but went in , and forgot the golden bird and his country in the same man
     into the wide world to seek for the golden bird ; but his father would not list
     till you come to a room , where the golden bird sits in a wooden cage ; close b
    age ; close by it stands a beautiful golden cage ; but do not try to take the bi
    t in and found the chamber where the golden bird hung in a wooden cage , and bel
     a wooden cage , and below stood the golden cage , and the three golden apples t
    tood the golden cage , and the three golden apples that had been lost were lying
     took hold of it and put it into the golden cage . but the bird set up such a lo
     unless he should bring the king the golden horse which could run as swiftly as 
     if he did this , he was to have the golden bird given him for his own . so he s
    , however , tell you how to find the golden horse , if you will do as i bid you 
    athern saddle upon him , and not the golden one that is close by it. ’ then the 
    m lay snoring with his hand upon the golden saddle . but when the son looked at 
     he deserves it. ’ as he took up the golden saddle the groom awoke and cried out
    very joyful ; and you will mount the golden horse that they are to give you , an
    t it , to see whether it is the true golden bird ; and when you get it into your
    

Similar() anvendes til at identificere ord, der optræder i en lignende kontekst.


```python
nltk_text.similar('golden')
```

    good shabby handsome leathern right
    

Dispersion_plot() anvendes til at visualisere, hvordan termer forekommer på tværs af vores tekst. Metoden accepterer en liste med et eller flere udtryk som input.


```python
terms = ['horse', 'bird', 'apples', 'cage']

nltk_text.dispersion_plot(terms)
```


    
![png](fig/output_21_0.png)
    


Generate() anvendes til at genere mere eller mindre sammenhængende tekst med udgangspunkt i en eksisterende tekst.


```python
text_gen = nltk_text.generate(150)
```

    out , and forgot the golden saddle . , that , if you will reach a
    village in the wind . fox met him , and all the council was called
    together . apples was missing . very fond of his son , and went home
    to the village where he had the princess wept . castle and pass on and
    on till you come to a wood , and was sentenced to die . it . . his
    country too . and put it into the smart house , and the king said , ‘
    your brothers have set watch to kill you , if they find you in the
    same manner . ‘ all this have we won by our labour. of two things ;
    ransom no one from the gallows , and the fox said , ‘ one feather is
    of no river. went his way ,
    

    Building ngram index...
    


```python

```



{% include links.md %}


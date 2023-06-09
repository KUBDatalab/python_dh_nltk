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




{% include links.md %}


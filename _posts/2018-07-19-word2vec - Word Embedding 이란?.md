---
layout : post
title : "word2vec - Word Embedding 이란?"
date : 2018-07-19 01:00:00 +0900
author : viking
category: PyTorch
tag: ['Deep Learning']
---

word embedding 기법 중 하나인 word2vec 을 포스팅하기 위한 밑밥 깔기 포스팅이다.

<br><br>

## Word Embedding 이란?

`Word Embedding` 은 word를 숫자로 변환한 것이다. 머신러닝, 딥러닝의 아키텍처에 데이터를 집어 넣을 때는 문자 그대로 집어넣는 것이 아니라 어떤 숫자, 벡터 형태여야 하기 때문에 Word Embedding 이 필요하다.

<br>
## Word Embedding 방법
Word Embedding 을 하는 방법은 여러가지이다.

<br>

- one hot encoding
- count vector
- tf-idf
- Co-Occurrence Matrix
- word2vec
- GloVe
- fasttext

<br>

등의 방법이 있다.
<br>

그렇다면 문자를 어떻게 숫자로 바꿀까? 아래에 간단한 예제를 작성해보았다.
<br>

```
import numpy as np

sentences = ["I bought Mamamoo Red moon album.", "I like Red Velvet", "I saw BTS and Red Velvet yesterday."]

word_dic = {}
idx = 0
for sentence in sentences:
    for word in sentence.split(' '):
        if word not in word_dic:
            word_dic[word] = idx
            idx += 1

print(word_dic)
```

<br>
위와 같이 "I bought Mamamoo Red moon album.", "I like Red Velvet.", "I saw BTS and Red Velvet yesterday." 이라는 세 문장이 있다고 해보자.

문장 별 단어 개수는

"I bought Mamamoo Red moon album."     =>  6개
"I like Red Velvet."                   =>  4개
"I saw BTS and Red Velvet yesterday."  =>  7개

로 총 17개가 있지만 문장에서 'I', 'Red', 'Velvet' 이 여러 문장에서 겹친다. 겹치는 단어들을 1개만 카운트 해서 단어들을 dict 로 표현하면 아래와 같다.


```
>> {'I': 0,
 'bought': 1,
 'Mamamoo': 2,
 'Red': 3,
 'moon': 4,
 'album.': 5,
 'like': 6,
 'Velvet.': 7,
 'saw': 8,
 'BTS': 9,
 'and': 10,
 'Velvet': 11,
 'yesterday.': 12}
 ```

<br>
이 단어들의 index 를 이용해서 원래 문장을 숫자로 표현하면 아래와 같다.
<br><br>


```
 one_hot_vector = []
 for sentence in sentences:
     tmp = []
     for word in sentence.split(' '):
         print(word, word_dic[word])
         tmp.append(word_dic[word])
     one_hot_vector.append(tmp)

print(one_hot_vector)
```

<br>

```
>> [[0, 1, 2, 3, 4, 5], [0, 6, 3, 7], [0, 8, 9, 10, 3, 11, 12]]
```

이런식으로 문장을 숫자로 표현할 수 있다. 문장에서 같은 단어가 쓰인 부분은 해당 단어를 표현하는 값이 같다는 것을 확인할 수 있다. 이런식으로 문장을 숫자로 바꿀 수 있다.

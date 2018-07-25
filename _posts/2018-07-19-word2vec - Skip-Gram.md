---
layout : post
title : "word2vec - Skip-Gram"
date : 2018-07-19 01:00:00 +1500
author : viking
category: Deep Learning
---
<script type="text/javascript"
       src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default">
</script>
## Skip Gram

skip-gram 은 "Great power comes great responsibility." 라는 문장
아래는 skip-gram 의 아키텍처다.

![Skip-Gram](https://dl.dropbox.com/s/hyf7umh4xrzol24/skip_gram.png?)


`V : 임베딩하려는 단어 수, N : 은닉층의 노드 수` 이므로 $V \times 1$ 크기의 one-hot vector 가 입력으로 사용된다. 따라서 가중치 행렬 $W$ 는 $V \times N$ 이 된다.

word2vec의 입력이 one-hot vector 이기 때문에 $W$의 행벡터가 각 단어에 해당하는 임베딩 단어벡터와 같다.

아래 처럼 [0 1 0 0 0] 인 one-hot-vector 가 $W$
$$
    \begin{bmatrix}
    0 & 1 & 0 & 0 & 0\\
    \end{bmatrix}
    \begin{bmatrix}
    3 & 27 & 5 \\
    10 & 9 & 6 \\
    12 & 7 & 8 \\
    2 & 5 & 4 \\
    1 & 3 & 8 \\
    \end{bmatrix} =
    \begin{bmatrix}
    10 & 9 & 6 \\
    \end{bmatrix}
$$

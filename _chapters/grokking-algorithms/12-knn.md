---
title: Chapter 12 - K-nearest neighbors
order: 12
date: 2026-07-21
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

- Building a classification system using k-nearest.

- Feature extraction.

- Using regression to predict a number.

- Limitations of the k-neighbors.


## Classifying oranges vs grapefruits


Imagine some fruit harvesting company is picking up different types of fruits, in this case oranges and grapefruits, and they carry them together but at some point they want an automated system to classify them.

Now there are clear differences between oranges and grape fruit, one of the most noticable being size and color, those are two good features (characteristics) of the items we are studying. 

Most of the time, a redder and bigger fruit will be a grape fruit, so for every fruit you cuold take a sample and generate the following graph:

![orange vs grapefruit features]({{ '/assets/images/grokking_algos_chapter_12/k-n.svg' | relative_url }}){: .center-img }


Imagine at some point we get some data measured in size and color of a new fruit we can't see it (someone told us), like here, which fruit should we think it is?

![fruit to classify]({{ '/assets/images/grokking_algos_chapter_12/k-n-to-classify.svg' | relative_url }}){: .center-img }


Well, being a 2 dimension graph, we should measure the euclidean distance to all the `k` closest neighbours and count how many elements belong to each class. The class with the most neighbours (orange or grapefruit) wins.

## Building a recommendation system

Similar to before, we have a problem where now we have users and have to figure out their taste on films so we can recommend them. We need to figure somewhat the distance (how similar they are) between users, and if one of two close users watches a movie and likes it, we can most likely recommend it to the other user.

Before the features were clear, size and color (redness) to classify between oranges and grapefruits; we had two features and moved into a two-dimensional plane, given two points we could compute the distance betwen them using eucledian distance, for example:

```python
# pseudo code
point_a = (2, 2)
point_b = (2, 1)
d = sqrt((point_a[0]-point_b[0])^2 + (point_a[1] - point_b[1])^2)
# whic is 1
```

Now, if we are checking netflix users... what features should we pick (picking the right features is key for performance). When we can map these features for each user in a plane we can then start computing distances. How does our movie recommendation system does it? Well we can ask them to rate some movie categories, those will be our features:


```markdown
| | Alice | Bob | Charles |
|--|------|-|-|
| COMEDY | 3 | 4 | 2|
|ACTION | 4|3|5|
|DRAMA|4|5|1|
|HORROR|1|1|3|
|ROMANCE|4|5|1|

```
So we could put the features in a plane like (note, those are called vectors):

```markdown
alice=(3,4,4,1,4)
bob=(4,3,5,1,5)
charles=(2,5,1,3,1)
```

Now the formula to compute the distance is the same, but slightly more complex. We compare every pair of matching features:

```markdown
sqrt((a_1-a_2)^2 + (b_1-b_2)^2 + ...)
```

And we end up having a single number, which is the distance, this small snippet:

```python
import math


def distance(
    user_1_v:  tuple[int, int, int, int, int],
    user_2_v: tuple[int, int, int, int, int],
) -> float:
    acc = 0
    for p1,p2 in zip(user_1_v,user_2_v,strict=True):
        acc += (p1-p2)**2
    return math.sqrt(acc)

def main() -> None:
    users = {
        "alice": (3, 4, 4, 1, 4),
        "bob": (4, 3, 5, 1, 5),
        "charlie": (2, 5, 1, 3, 1),
    }
    dist_a_b = distance(user_1_v=users['alice'],user_2_v=users["bob"])
    dist_a_c = distance(user_1_v=users['alice'], user_2_v=users["charlie"])
    dist_b_c = distance(user_1_v=users["bob"],user_2_v=users["charlie"])
    print(f"Distance between Alice and Bob {dist_a_b}.")
    print(f"Distance between Alice and Charlie {dist_a_c}.")
    print(f"Distance between Bob and Charlie {dist_b_c}.")



if __name__ == "__main__":
    main()
```

So we get:

```markdown
Distance between Alice and Bob 2.0.
Distance between Alice and Charlie 4.898979485566356.
Distance between Bob and Charlie 6.6332495807108.
```

Alice and Bob are the most similar users.


## Regression

Now instead of predicting what category something will fall into, we want to predict a numeric value, such as a rating. Imagine that for the movie Deadpool we have seen some users rate it as follows:

```markdown
- user 1: 5
- user 2: 4
- user 3: 4
- user 4: 5
- user 5: 3
```

If we select the `k` users closest to the user we are predicting for, we can average their ratings and use that as the prediction. For example, averaging all five ratings gives `4.2`, but KNN regression normally averages only the ratings from the selected nearest neighbours.

These are two characterstics that KNN doe:

- Classifcation: Categorize into a group.
- Regression: Predicting a response.

A more complex example, we have a bakery and we produce bread every day. We want to predict how much we will produce in day.

Great, but how? Again here features come into play, we need to know which features are relevant (PCA, another topic) and measure them, build a dataset.

After some study we decide that these are the features we will build our dataset with:

- Weather on a scale 1 to 5, bad to great.
- Weekend or holiday, 1 or 0.
- Is there a game on, 1 or 0 for yes/no.

Imagine we have gathered the following dataset:

```markdown
| day | sold| weather|is_weekend|is_game|
|-|-|-|-|-|
|1|300|5|1|0|
|2|225|3|1|1|
|3|75|1|1|0|
|4|200|4|0|1|
|5|150|4|0|0|
|6|50|2|0|0|

```
Because of the small number of rows and features you can still see some features, but what will happen when we feed this vector to our algorithm `(4,1,0)`?

- Weather is somewhat good at 4.
- Is it weekend.
- There is no game.

We compute the squared Euclidean distance to every day and obtain:

```markdown
| day | distance|
|-|-|
|1|1|
|2|3|
|3|9|
|4|2|
|5|1|
|6|5|
```

Days 1, 2, 4, and 5 are the closest (`k=4`), so if we average their sales we predict `218.75`.


#### A note on Cosine similarity

So far we used euclidean distance. Another common measure is cosine similarity, which compares the angle between vectors instead of their distance. A higher cosine similarity means that two vectors are more similar.


### Picking good features

Its key as mentioned, we have to take into account some things:

- Features should usually be normalized or standardized, otherwise a feature with larger numeric values can dominate the distance.

- The value of `k` matters. A small `k` is more sensitive to noise, while a large `k` produces smoother predictions but can miss local patterns.

- Directly correlate (or we think of) with the thing we are trying to recommend.
- That they are not biased, e.g only asking users to rate comedy movies we can't knwo if they like aciton movies.


## Introduction to machine learning

### OCR

Otpical Character Recognition, you take a picture of something and are able to read the text, e.g a number.

We can use KNN to figure what number it is:

1. We train though a lot of images to extract the features of te numbers.
2. You get a new image, you see which cluster (number) is closest to.

E.g nu ber `3` has two curves, 7 has like some edges.

In this case feature extraction, what is relevant, is more complicated than the orange and grapefruits.



## Closing up

- KNN is used for classification and regressiom by looking at the k-nearest neighbours.

- Classification is to categorize into a group and regression is to predict a response.

- Feature extraction, what our model will be using, is crucial.


To remember:

- In classification, KNN looks at the most common class among the closest neighbours; in regression, it averages their numeric values.

- Euclidean distance measures the distance between points, while cosine similarity compares the angle between vectors.

- Features should be normalized or standardized so features with larger numeric ranges do not dominate the distance.

- The value of `k` matters: small values are more sensitive to noise, while larger values produce smoother predictions but can miss local patterns.

- KNN is a lazy learner: it does little work during training and calculates distances when making a prediction.

- Irrelevant features and too many dimensions can make neighbours less meaningful and lead to incorrect predictions.

---
title: "Recurrent Neural Networks for Song Lyrics"
date: 2019-05-05
draft: false
---

## Introduction

I recently became interested in neural networks and sought interesting applications of what they could do. Neural networks are based on how the human brain works; the networks are made up of many nodes that take inputs and predict outputs. Over time, with more data, the networks improve by correcting for error, known as backpropagation. Neural networks become especially useful when there are many “layers” between the input and output that allow for more fine-tuned fitting of data.

Recurrent neural networks have a sense of sequence built in, making them good for predicting text, market prices, or any other ordered data. Andrej Karpathy has written an [easy-to-use module](https://github.com/karpathy/char-rnn) for recurrent neural networks that uses Torch, which I used for my neural network exploration.

I set out to predict song lyrics for a given artist by compiling their lyrics and feeding it into the neural network. In order to do this, I first built a Python script that searched for and combined the lyrics for an artist into a single file. I used the [musixmatch API](https://developer.musixmatch.com/) for gathering the data. I first gathered the track ids in batches of 20:

```
def get_track_ids(artist):
    page_number = 1
    track_ids = []
    while True:
        url = "track.search?q_artist=" + artist + "&page_size=20&page=" + str(page_number) + "&s_track_rating=desc"
        response = call_api(url)
        if response["message"]["header"]["status_code"] == 400:
            break
        track_list = response["message"]["body"]["track_list"]
        for track in track_list:
            track_ids.append(track["track"]["track_id"])
        page_number += 1
    return track_ids
```

Then iterated through the track ids and got the lyrics:

```
def get_lyrics(track_id):
     url = "track.lyrics.get?track_id=" + str(track_id)
     response = call_api(url)
     return response["message"]["body"]["lyrics"]["lyrics_body"]
```

And wrote them all to a single file. I then ran it through a 2-layer recurrent neural network. Here are some snippets of the output data:

**Beatles lyrics:**
> If I needed someone of like girlen’s going look, oh my baby

> I just have to the someout in my mad

> Said at the band’s finey

> I need even my love babe

**Drake lyrics:**
> But the city, she a mix for me,

> Hey there yeah

> It’s tryna keep?

> (Oh my last me)

> In we said que this ash-we’re you, exsed to ya

**Gucci Mane lyrics:**
> Gucci baby-hood,

> Like your girlfriends, you can’t keep up

> I get that break of it, I just mean

While this couldn’t replace songwriters (and the lyrics only half make sense), the lyrics for each artist are clearly distinct and easily identifiable as the style of the artist.


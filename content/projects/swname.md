---
title: "Star Wars Name Generator"
description: "A small python program that generates new names similar to the current names that you feed it."
slug: "sw-name"
image: "sw.png"
tags: []
keywords: ""
categories:

    - "Name generator"
    - "Star Wars"

date: 2017-12-05T00:49:29-06:00
draft: false
---
# Name Generator

## Description

This is a python program I wrote to create random names from the names that you feed it. The more names you feed it the more interesting the generated names are.
The program uses a simple formula to generate pseudo random numbers from a seed. You can change the seed to get different names. There is no user interface on top of this application so just copy the source code and change the names to run it.

## Implementation

I created a class called `Random` to initialize the random numbers with a given seed:

```python
class Random:
    def __init__(self, seed): # Initalizes Random object
        self.seed = seed
    def next(self, limit):
        # Use the formula to generate the random number
        self.seed = ((16807 * self.seed) % 2147483647)
        nSeed = self.seed % limit
        return nSeed
    def choose(self, characters):
        # call next and choose a character from the string characters
        i = self.next(len(characters))
        c = characters[i]
        return c
```

You can create a new set of random numbers by creating a new object of type Random like `random = Random(seed)`.

I created a class called `Words` to create a dictionary of randomly generated words and a function to display a new generated word:

```python
class Words:
    def __init__(self, seed): # Initalizes Words object
        self.first = ''
        self.follow = {} # Dictionary
        self.random = Random(seed)
    def add(self, word):
        # define add function here.
        self.first = self.first + word[0]
        for i in range(0, len(word) - 1):
            if word[i] in self.follow:
                self.follow[word[i]] = self.follow[word[i]] + word[i+1]
            else:
                self.follow[word[i]] = word[i+1]
        return None
```

The add function adds a single word to the dictionary of strings. You can call this function as many times as you want to keep adding words to the dictionary.
The class `Words` is also used to pass the seed to the `Random` class. I used a seed of 501 and I also added character's names from Star Wars:

```python
star = Words(501) # change the seed to get different names.
star.add('Luke')
star.add('Leia')
star.add('Anakin')
star.add('Padme')
star.add('Rey')
star.add('Kylo')
star.add('Han')
star.add('Greedo')
star.add('Obi-wan')
star.add('Qui-gon')
star.add('Maul')
star.add('Bane')
star.add('Revan')
star.add('Kit')
star.add('Mace')
star.add('Boba')
star.add('Jango')
star.add('Sidious')
star.add('Vader')
star.add('Plagueis')
star.add('Sion')
```

Finally I created a `make` function inside of the class `Words` to create a random word from the dictionary created by the `add` function. The make function chooses a letter from the first letter of a random word in the dictionary and then chooses another word and selectes another letter until it creates a word that hits the passed in size. Here is the code for the `make` function:

```python
# This is inside the class Words
def make(self, size):
    # define make function here
    i = 0
    fLetter = self.random.choose(self.first)
    word1 = fLetter
    nLetter = fLetter
    while nLetter in self.follow and i < size:
        nLetter = self.random.choose(self.follow[nLetter])
        word1 = word1 + nLetter
        i = i + 1
    return word1
```

We can print the output of the make function multiple times with different sizes to return many words. I put four make functions in a loop:

```python
# Makes 4 names of varying length each time it goes through the loop.
i = 0
while i <= 40:
    # Change in the integer in the while loop to make more or less names
    print(star.make(5))
    print(star.make(7))
    print(star.make(4))
    print(star.make(6))
    i = i + 4
```

This will give me 40 different names. The output I got was interesting but there was some good names to choose from.

*Try this with your favorite characters and see what happens*

Here was my final output:

```python
# This is the output my program produces at default values.
# Lulang
# Kyladone
# Jakei
# Lui-gon
# Si-wad
# Jadonana
# Hakin
# Reionan
# Greian
# Maulagon
# Manau
# Leyloui
# Lulone
# Rereeylo
# Aneis
# Kylonei
# Mauki-
# Sidongob
# Magon
# Hangona
# Pladme
# Si-wacee
# Pango
# Mananan
# Vagong
# Reidonad
# Leree
# Vaneede
# Reingo
# Greylona
# Lueyl
# Qukeis
# Lus
# Hadei-go
# Kylag
# Paceeva
# Aneylo
# Greerere
# Lerey
# Lus
# Reylon
# Ploukeio
# Madey
# Qus
```

Full source code:

```python
# Marcus Virginia - virgi019
class Random:
    def __init__(self, seed): # Initalizes Random object
        self.seed = seed
    def next(self, limit):
        # Use the formula to generate the random number
        self.seed = ((16807 * self.seed) % 2147483647)
        nSeed = self.seed % limit
        return nSeed
    def choose(self, characters):
        # call next and choose a character from the string characters
        i = self.next(len(characters))
        c = characters[i]
        return c

class Words:
    def __init__(self, seed): # Initalizes Words object
        self.first = ''
        self.follow = {} # Dictionary
        self.random = Random(seed)
    def add(self, word):
        # define add function here.
        self.first = self.first + word[0]
        for i in range(0, len(word) - 1):
            if word[i] in self.follow:
                self.follow[word[i]] = self.follow[word[i]] + word[i+1]
            else:
                self.follow[word[i]] = word[i+1]
        return None
    def make(self, size):
        # define make function here
        i = 0
        fLetter = self.random.choose(self.first)
        word1 = fLetter
        nLetter = fLetter
        while nLetter in self.follow and i < size:
            nLetter = self.random.choose(self.follow[nLetter])
            word1 = word1 + nLetter
            i = i + 1
        return word1
```
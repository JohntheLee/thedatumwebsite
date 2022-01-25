---
title: Adventures in GIS, Episode 6
author: 'Jeonghoon Lee'
summary: The sixth episode to my trek through learning about GIS software
date: '2022-01-25'
slug: []
categories: []
tags: []
Description: ''
Tags: [ArcGIS, ArcMAP, Ryerson]
Categories: [ArcGIS, ArcMAP, Ryerson]
DisableComments: no
---

Welcome to the sixth episode of my journey through GIS software. I recently had the opportunity to learn about Python customization and ArcGis script construction. The first half of this class was mainly review regarding programming principles and etiquette. Some of the script structures and terminology that Python programmers use are very similar to those employed by R programmers. I enjoyed learning about building for/while loops in the Python language. However, I was made aware of the fact that Python doesn't vectorize its arrays. Now I can finally understand why Python/R users advertise importing the Numpy/Reticulate packages.

The second half of the class focused on working with Arcpy. How can GIS technicians leverage their knowledge of Python coding to their advantage? Arcpy lets GIS users build their own custom scripts, bring it into their GIS software interface, and incorporate their custom code into their GIS workflow. Many of the techniques that I was taught (eg. cursors, conditional loops) proved to be handy in later ArcGis projects. I am now able to rasterize, reclassify, and combine multiple vector maps with one script.

The course itself was enriching. However, I often suspected that I was one of few students actually taking the class. The exercises and workshops were fair and the directions for the lab assignments were well laid out. I suspect that the stereotype of "difficult computer coding course" might have intimidated potential students. Unfortunately, I will not be posting my code to the large lab assignments as that may violate some academic integrity rules. However, here is a snippet of some code that I wrote for a geoguessing game that we were assigned. The "countries_username" package is custom made. The code was written in Python 3.7.10.

Please look forward to the next episode where I talk about the environmental principles in GIS course.

```{python}
# Geoguessing game workshop

import csv
import random
import countries_username

fullname = countries_username.username()

file_handle = open("C:\\Users\\John Lee\\Desktop\\GIS Cert\\CODG132\\Countries.csv")
reader = csv.DictReader(file_handle)
countries = list(reader)
file_handle.close()

randomcountry = random.choice(countries)
print(randomcountry)

guess = ""
attempts = 0
attempted_guess = []

while guess != randomcountry["country"] and attempts < 10:
    guess = input("What country am I? ").casefold()
    attempts += 1
    attempted_guess.append(guess)

    if guess.casefold() == randomcountry["country"].casefold():
        print("Well done! You successfully identified the country!")
        break
    if attempts >= 10:
        print("You're fresh out of guesses")
        break
    
    for country in countries:
        
        if guess.casefold() == country["country"].casefold():
            if float(randomcountry["lat"]) > float(country["lat"]):
                print("I'm north of " + guess)
            else:
                print("I'm south of " + guess)
                
            if float(randomcountry["long"]) > float(country["long"]):
                print("I'm east of " + guess)
            else:
                print("I'm west of " + guess)

countries_fullname = "Your full name is " + str(fullname) + "."
countries_line = "The country was " + randomcountry["country"] + "!"
countries_guess = "Your guess(es) was/were: " + ", ".join(attempted_guess) + "."
countries_attempt = "It took you " + str(attempts) + " attempts."

countries_log = open("countries_log.txt", "w+")
countries_log.write("Welcome to the countries geoguessing game log file.")
countries_log.write("\n")
countries_log.write(countries_fullname)
countries_log.write("\n")
countries_log.write(countries_line)
countries_log.write("\n")
countries_log.write(countries_guess)
countries_log.write("\n")
countries_log.write(countries_attempt)
countries_log.close()
```
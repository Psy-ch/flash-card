# Build a Flashcards App With Django
by Angelene Santosh  August, 2022 

## Table of Contents

- Demo: Your Django Flashcards App
- Project Overview
- Prerequisites
- Step 1: Prepare Your Django Project
    - Create a Virtual Environment
    - Add Dependencies
    - Initiate Your Django Project
- Step 2: Set Up Your Flashcards App
    - Create Your Django Flashcards App
    - Launch Your Landing Page
    - Sprinkle in Some Style
- Step 3: Reveal Your Cards
    - Connect the SQLite Database
List Your First Flashcards
Adjust Your Templates
Step 4: Get Into the Card Details
Create a New Card
Update an Existing Card
Connect Your Pages
Step 5: Check Your Cards
Make Your Cards Move
Simulate a Card Check Session
Step 6: Put Your Cards in Boxes
Show a Box
List Your Boxes
Pick a Random Card
Check a Card
Conclusion
Next Steps

Flashcards are a great tool when you want to memorize a new topic or learn a new language. You write a question on the front of the card and the answer on the back of the card. Then you can test your memory by going through the flashcards. The more often you show a card to yourself, the better your chances of memorizing its content. With Django, you can build your own flashcards app.

By following this tutorial, you’ll build a Django flashcards app that replicates a spaced repetition system, which can boost your learning potential.

In this step-by-step project, you’ll learn how to:

- Set up a Django project
- Work with a SQLite database and the Django shell
- Create models and class-based views
- Structure and nest templates
- Create custom template tags
Your Django Flashcards app
On the front page of the web app, exisiting cards will be shown and you can create new ones.
On each flashcard,you can add a question and an answer which can be edited later.
The card advances to the following box once you have determined the answer to its question. Flashcards do not automatically end when they are moved to the next box. It will still advance through the boxes when you examine it sometimes to refresh your memory. In general, you are more likely to have mastered those topics if your box number is higher. A card returns to the first box if you don't know the response to its query.

## A PROJECT OVERVIEW:
A full-stack web app with a database connection that replicates the Leitner system.
The card advances to the following box once you have determined the answer to its question. Flashcards do not automatically end when they are moved to the next box. It will still advance through the boxes when you examine it sometimes to refresh your memory. In general, you are more likely to have mastered those topics if your box number is higher. A card returns to the first box if you don't know the response to its query.

By using spaced repetition, you’ll test your knowledge of the new or challenging topics in the first box more frequently, while you’ll check the cards from the other boxes in larger time intervals:

- You have five boxes that can contain flashcards.
- When you create a flashcard, you put it into the first box.
- To test your knowledge, you choose a box, pick a random flashcard, and check if you know the answer to the card’s question.
- If you know the answer, then you move the card to the next higher box.
- If you don’t know the answer, then you move the card back to the first box.
- The higher the box number, the less frequently you check the flashcards in that box to test your knowledge.
The higher the box number, the less frequently you check the flashcards in that box to test your knowledge.

## Prerequisites
Previous knowledge of Django or databases is not required for this project. However,one should be comfortble using command line and have basic knowledge of Python and classes.
It also helps to know about virtual environments and pip so it's easier to get started on the project.

## Step 1: Preparing Djano project
Here, Firstly a virtual environment should be created and all dependencies(Django) should be installed. Following are the steps:
1. Go to the terminal and create a directory
```
$ mkdir flash_card
$ cd flash_card
```
2. Select operating system and use platform-specific command to set up a virtual environment
```
$ python3 -m venv venv
$ source venv/bin/activate
(venv)$
```
3. Add dependencies
``` 
(venv)$ python -m pip install django==4.0.4
```
3. Intiate the Django project
```
(venv)$ django-admin startproject flashcards .
```
4. To run the web server
```(venv)$ python manage.py runserver
```
With the server runnning, one can visit their project in browser by using() http://127.0.0.1:8000 or http://localhost:8000:_
## Step: 2 Set up your flashcards app

1. Creating the Django flashcard app
You need only one app besides your project. The primary purpose of that app is to handle your app’s cards, so you can call the app cards. Run the command to create the cards app:
```
(venv)$ python manage.py startapp cards
```
This command creates a cards/ folder in your project, with some predefined files. 
To connect the cards app to the flashcards project, add it to INSTALLED_APPS in flashcards/settings.py:

flashcards/settings.py

INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "cards.apps.CardsConfig",
]



2. Launching your landing page
So far, Django still shows the jiggling rocket on the landing page of your flashcards project. In this section, you’ll implement your custom landing page by using a base template.Open urls.py in your flashcards/ folder and include your cards URLs:
flashcards/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("cards.urls")),
]


First, you need to tell Django that your cards app takes care of the root URL of your project now.
You also pass in name="home" as an optional argument. With a name, you can reference views conveniently in your Django project. So even if you decide to change the URL pattern at some point, you don’t have to update any templates.




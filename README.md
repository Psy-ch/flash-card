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
    - List Your First Flashcards
    - Adjust Your Templates
- Step 4: Get Into the Card Details
    - Create a New Card
    - Update an Existing Card
    - Connect Your Pages
- Step 5: Check Your Cards
    - Make Your Cards Move
    - Simulate a Card Check Session
- Step 6: Put Your Cards in Boxes
    - Show a Box
    - List Your Boxes
    - Pick a Random Card
    -Check a Card
- Conclusion
- Next Steps

Flashcards are a great tool when you want to memorize a new topic or learn a new language. You write a question on the front of the card and the answer on the back of the card. Then you can test your memory by going through the flashcards. The more often you show a card to yourself, the better your chances of memorizing its content. With Django, you can build your own flashcards app.

By following this tutorial, you’ll build a Django flashcards app that replicates a spaced repetition system, which can boost your learning potential.

![Flash Card Screenshot](https://github.com/Psy-ch/flash-card/blob/main/img/flash_card_screen_01.png)


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
``` shell
$ mkdir flash_card
$ cd flash_card
```
2. Select operating system and use platform-specific command to set up a virtual environment
```shell
$ python3 -m venv venv
$ source venv/bin/activate
(venv)$
```
3. Add dependencies
``` shell
(venv)$ python -m pip install django==4.0.4
```
3. Intiate the Django project
``` shell
(venv)$ django-admin startproject flashcards .
```
4. To run the web server
``` shell
(venv)$ python manage.py runserver
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

``` python 
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
```

2. Launching your landing page
So far, Django still shows the jiggling rocket on the landing page of your flashcards project. In this section, you’ll implement your custom landing page by using a base template.

``` python
flashcards/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("cards.urls")),
]
```

First, you need to tell Django that your cards app takes care of the root URL of your project now.

With this URL pattern in place, Django passes on any URLs of your project, except admin/, to your cards app. To handle this, your cards app needs its own urls.py file, which takes over the responsibility of dispatching URLs:
``` python
 # cards/urls.py

from django.urls import path
from django.views.generic import TemplateView

urlpatterns = [
    path(
        "",
        TemplateView.as_view(template_name="cards/base.html"),
        name="home"
    ),
]
```
To serve the template that you’re referencing to, create base.html in cards/templates/cards/:

``` html
<!-- cards/templates/cards/base.html -->

<!DOCTYPE html>
<html lang="en">

<head>
    <title>Flashcards</title>
</head>

<body>
    <header>
        <h1>Flashcards App</h1>
    </header>
    <main>
        {% block content %}
            <h2>Welcome to your Flashcards app!</h2>
        {% endblock content %}
    </main>
</body>

</html>
```
Hop over to your browser and visit http://127.0.0.1:8000 by typing the following on the terminal:
``` 
(venv) $ python manage.py runserver
```
![Flash Card Screenshot](https://github.com/Psy-ch/flash-card/blob/main/img/flash_card_screen_02.png?raw=true)

You can add design to your HTML pages by using CSS. Instead of writing all the CSS code yourself, you can import an external CSS file:
``` html
<!-- cards/templates/cards/base.html -->

<!DOCTYPE html>
<html lang="en">

<head>
    <title>Flashcards</title>
    <link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">
</head>

<!-- ... -->
```

Go ahead and restart your development web server. Then, visit your flashcards app at http://127.0.0.1:8000:
![Flash Card Screenshot](https://github.com/Psy-ch/flash-card/blob/main/img/flash_card_screen_03.png?raw=true)

## Step 3: Reveal your cards
Once your app is operational, you can specify the design of the SQLite database's tables. To accomplish this, include a Card model.

1. Connect the SQLite Database
The front of flashcards is typically where you ask a question. The solution is printed on the back of the card. You can imitate a card's characteristics in your cards app's model by doing the following:
``` python
# cards/models.py

from django.db import models

NUM_BOXES = 5
BOXES = range(1, NUM_BOXES + 1)

class Card(models.Model):
    question = models.CharField(max_length=100)
    answer = models.CharField(max_length=100)
    box = models.IntegerField(
        choices=zip(BOXES, BOXES),
        default=BOXES[0],
    )
    date_created = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.question

```
To update your database, you must create migrations and migrate the changes to apply them:
```
venv) $ python manage.py makemigrations
Migrations for 'cards':
  cards/migrations/0001_initial.py
    - Create model Card

(venv) $ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, cards, contenttypes, sessions
Running migrations:
  Applying cards.0001_initial... OK
  ...
```
You must add data to your database after making the necessary adjustments. You're going to insert some cards into your database for this purpose using the Django shell. Head over to the terminal and start the Django shell:
```
(venv) $ python manage.py shell
```
Within the shell, you can now interact directly with your flashcards project. Go ahead and create three cards:

```
>>> from cards.models import Card
>>> c1 = Card(question="Hello", answer="Hola")
>>> c1.save()
>>> c2 = Card(question="Please", answer="Por favor")
>>> c2.save()
>>> c3 = Card(question="Sorry", answer="Lo siento", box=2)
>>> c3.save()
>>> Card.objects.all()
<QuerySet [<Card: Hello>, <Card: Please>, <Card: Sorry>]>
```
2. List Your First Flashcards
Now it’s time to list your cards in the front end.

Start by writing a class-based view that lists all the cards:

``` python
# cards/views.py

from django.views.generic import (
    ListView,
)

from .models import Card

class CardListView(ListView):
    model = Card
    queryset = Card.objects.all().order_by("box", "-date_created")
```

Before you can look at your cards in the browser, you must define a template. Django expects the templates for class-based views to be in a specific location with a particular name. For your CardListView, you need a template named card_list.html in cards/templates/cards/:
``` html
<!-- cards/templates/cards/card_list.html -->

{% extends "cards/base.html" %}

{% block content %}
    <h2>
        All Cards
    </h2>
    {% for card in card_list %}
        <h3>🗃 {{ card.box }} Box</h3>
        <article>
            {{ card }}
        </article>
    {% endfor %}
{% endblock content %}
```
It makes sense for a flashcards app to list all the flashcards on your home page. Replace the former landing page with a URL to your new CardListView, and rename it to card-list:
``` python
cards/urls.py

from django.urls import path
# Removed: from django.views.generic import TemplateView

from . import views

urlpatterns = [
    path(
        "",
        views.CardListView.as_view(),
        name="card-list"
    ),
]
```
Restart your development web server and visit http://127.0.0.1:8000:

You can achieve this ordinal numbering with Django’s humanize template filters. This set of filters helps make data in your template more human-readable. To use it, you must add django.contrib.humanize to your INSTALLED_APPS:
```python  
# flashcards/settings.py

# ...


INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "django.contrib.humanize",
    "cards.apps.CardsConfig",
]

# ...
```


After you’ve adjusted your Django settings, you can load the humanize template filter in your card_list.html template:

``` html
<!-- cards/templates/cards/card_list.html -->

{% extends "cards/base.html" %}
{% load humanize %}

<!-- ... -->
```
Stay in card_list.html and wrap your <h3> headline in an ifchanged template tag to show your new box headlines only once:

``` html
<!-- cards/templates/cards/card_list.html -->

<!-- ... -->

{% for card in card_list %}
    {% ifchanged %}
        <h3>🗃 {{ card.box | ordinal }} Box</h3>
    {% endifchanged %}
    <article>
        {{ card }}
    </article>
{% endfor %}

<!-- ... -->
```
 adjust card_list.html to include the upcoming card.html template:

``` html
<!-- cards/templates/cards/card_list.html -->

<!-- ... -->

{% for card in card_list %}
    {% ifchanged %}
        <h3>🗃 {{ card.box | ordinal }} Box</h3>
    {% endifchanged %}
    {% include "cards/card.html" %}
{% endfor %}

<!-- ... -->
```
Now your card_list.html template will load a subtemplate named card.html. This template doesn’t exist yet. Go on and create card.html:
``` html 
<!-- cards/templates/cards/card.html -->

<article>
   <h4>{{ card.question }}</h4>
   <p>{{ card.answer }}</p>
</article>
```

Restart your development web server and visit http://127.0.0.1:8000:
[Flash_card](https://github.com/Psy-ch/flash-card/blob/main/img/flash_card_screen_05.png?raw=true)

## Step 4 : Get into card details
1. Create a New Card
The procedure you used to list your cards is comparable to how you implement the feature of adding a new card in the front end. You first make a view, then you add a route, and then you make a template.

The view is once more one of Django's generic views. You import a CreateView this time to base your CardCreateView off of:
``` python 
# cards/views.py

from django.urls import reverse_lazy
from django.views.generic import (
    ListView,
    CreateView,
)

from .models import Card

class CardListView(ListView):
    model = Card
    queryset = Card.objects.all()

class CardCreateView(CreateView):
    model = Card
    fields = ["question", "answer", "box"]
    success_url = reverse_lazy("card-create")

```
When you send the form, Django will check the form and return your request to the URL that you set for success_url in line 18. In this case, it’s the same view again. That way, you can create one card after another without navigating back and forth.

Go on and create it:
 ``` python
 cards/urls.py

# ...

urlpatterns = [
    # ...
    path(
        "new",
        views.CardCreateView.as_view(),
        name="card-create"
    ),

]
```
With this new path, your flashcards app has a new route that connects to CardCreateView.

When you now visit http://127.0.0.1:8000/new, Django serves you the CardCreateView but can’t find a corresponding template. Django looks for a template named card_form.html. This template doesn’t exist yet, so you need to create it:

``` html
<!-- cards/templates/cards/card_form.html -->

{% extends "cards/base.html" %}

{% block content %}
    <h2>✨ Create New Card</h2>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <input type="submit" value="Save Card">
    </form>
    <a href="{% url 'card-list' %}">
        Cancel
    </a>
{% endblock %}
```
2. Update an Existing Card
Chances are, you thought of a layout similar to your creation page, only the form is filled out with the editable content of your card. Create CardUpdateView, which takes advantage of this similarity:

``` python
# cards/views.py

from django.urls import reverse_lazy
from django.views.generic import (
    ListView,
    CreateView,
    UpdateView,
)

from .models import Card

# ...

class CardUpdateView(CardCreateView, UpdateView):
    success_url = reverse_lazy("card-list")
```
Instead of returning to the same page as you do in card-create, you’ll return to the card-list page after editing a card.

Create a new route to access a card’s edit page:
``` python
# cards/urls.py

# ...

urlpatterns = [
    # ...
    path(
        "edit/<int:pk>",
        views.CardUpdateView.as_view(),
        name="card-update"
    ),
]
```
Now you need to adjust card_form.html to cater to both conditions, creating a new card and editing an existing card:

``` html
<!-- templates/cards/card_form.html -->

{% extends "cards/base.html" %}

{% block content %}
    {% if card %}
        <h2>✏️ Edit Card</h2>
    {% else %}
        <h2>✨ Create New Card</h2>
    {% endif %}
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <input type="submit" value="Save Card">
    </form>
    <a href="{% url 'card-list' %}">
        Cancel
    </a>
{% endblock %}
```
When card data is present, Django will fill out your form with your card’s data without the need for any adjustments in the template.

3. Connect Your Pages
With your new routes in place, it’s time to connect your pages. Start by adding a link that leads to your card-create page from your card_list.html template:
``` html
<!-- templates/cards/card_list.html -->

{% extends "cards/base.html" %}
{% load humanize %}

{% block content %}
    <h2>
        All Cards
    </h2>
    <a href="{% url 'card-create' %}" role="button">
        ✨ Create New Card
    </a>
    {% for card in card_list %}
        {% ifchanged %}
            <h3>🗃 {{ card.box | ordinal }} Box</h3>
        {% endifchanged %}
        {% include "cards/card.html" %}
    {% endfor %}
{% endblock content %}
```
Next, show an Edit button for each card. Open your card.html template and add a link at the bottom:
``` html
<!-- templates/cards/card.html -->

<article>
    <h4>{{ card.question }}</h4>
    <p>{{ card.answer }}</p>
    <hr>
    <a href="{% url 'card-update' card.id %}" role="button">
        ✏️ Edit Card
    </a>
</article>
```
As your project grows, navigating between your pages becomes more and more important. So, another improvement to your flashcards app is a navigation menu.

Start by creating the navigation.html template with a link to your card-list page:
``` html
<!-- cards/templates/cards/navigation.html -->

<nav>
    <a href="{% url 'card-list' %}">🗂 All Cards</a>
</nav>
```
Include navigation.html in your base template to display it on all your pages:
``` html
<!-- cards/templates/cards/base.html -->

<!--  ... -->

<header>
    <h1>Flashcards App</h1>
    {% include "cards/navigation.html" %}
</header>

<!--  ... -->
```
## Step 5:Check Your Cards
In this step, you’ll implement the functionality to check if you know the answer to a card’s question. When you’ve memorized it correctly, you can move the card into the next box. If you don’t know the answer, then you move the card to the first box.

After implementing this vital feature, you’ll use the Django shell to see it in action. That way, you can verify that everything works as expected before writing your front-end code.

1. Make your cards move
You move a card to the next box if you recall its answer. If you don’t know the answer, you put the card back into the first box.

To replicate the behavior of moving a card between boxes, add a .move() method to your Card model:
``` python
# cards/models.py

# ...

class Card(models.Model):

    # ...

    def move(self, solved):
        new_box = self.box + 1 if solved else BOXES[0]

        if new_box in BOXES:
            self.box = new_box
            self.save()

        return self
```
Your .move() method takes solved as an additional argument. The value of solved will be True when you know the answer and False when you don’t.

2. Simulate a Card Check Session
Now that you’ve added .move() to your Card model, you can check if moving the cards works as expected. As you did earlier in this tutorial, use the Django shell:
``` python
(venv) $ python manage.py shell
```
You can now interact directly with your flashcards project and simulate a check session within the shell. First, import your Card model:
``` python
>>> from cards.models import Card
```
With the Card model imported, you can query your database to get all the cards from the first and second boxes:
``` python
>>> box1_cards = Card.objects.filter(box=1)
>>> box1_cards
<QuerySet [<Card: Hello>, <Card: Please>]>

>>> box2_cards = Card.objects.filter(box=2)
>>> box2_cards
<QuerySet [<Card: Sorry>]>
```
At the moment, box1_cards contains two cards, and box2_cards contains one card. Select the first card of box1_cards and move it to the next box:

```python
>>> check_card = box1_cards.first()
>>> check_card.move(solved=True)
<Card: Hello>

>>> box2_cards
<QuerySet [<Card: Hello>, <Card: Sorry>]>
```
When you call check_card.move() with solved set to True, then your card moves to the next box. The QuerySet of box2_cards now also contains your check_card.

For testing purposes, move the card further:
``` python
>>> check_card.move(solved=True)
<Card: Hello>

>>> box2_cards
<QuerySet [<Card: Sorry>]>

>>> check_card.box
3

```
Now, test what happens when you don’t know the answer to your card’s question:
```python
>>> check_card.move(solved=False)
<Card: Hello>

>>> check_card.box
1
```
When you call .move() with solved set to False, the card moves back to box one. 

## Step 6: Put your cards in Boxes
1. Show a box
To test your knowledge, you need to be able to pick a box for a learning session. That means you need a view for a single box, a route to navigate to, and a template to show the box.

Start by creating the view for a single box:
``` python
# cards/views.py

# ...

class BoxView(CardListView):
    template_name = "cards/box.html"

    def get_queryset(self):
        return Card.objects.filter(box=self.kwargs["box_num"])

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["box_number"] = self.kwargs["box_num"]
        return context

```
Add the new route to your urls.py file to accept box_num in the URL pattern:
```html
# cards/urls.py

# ...

urlpatterns = [
    # ...
    path(
        "box/<int:box_num>",
        views.BoxView.as_view(),
        name="box"
    ),
]
```
Next, you create the box.html template that your view is expecting:

``` html
<!-- templates/cards/box.html -->

{% extends "cards/base.html" %}
{% load humanize %}

{% block content %}
    <h2>🗃 {{ box_number | ordinal }} Box</h2>
    <mark>{{ object_list | length }}</mark> Card{{ object_list | pluralize }} left in box.
    <hr>
    {% for card in object_list %}
        {% include "cards/card.html" %}
    {% endfor %}
{% endblock content %}
```
You can visit your boxes directly by entering their URL in the browser’s address bar. But it’d be much more convenient to have links to your boxes in the navigation menu of your flashcards app.

2. List Your Boxes
First, create the template that you want to load:
``` html
<!-- cards/templates/box_links.html -->

{% load humanize %}

{% for box in boxes %}
    <a href="{% url 'box' box_num=box.number %}">
        🗃 {{ box.number | ordinal }} Box <mark>{{ box.card_count }}</mark>
    </a>
{% endfor %}
```
 Then, create a folder named templatetags/ next to your templates/ folder. Then, create cards_tags.py inside the templatetags/ folder:
``` python
# cards/templatetags/cards_tags.py

from django import template

from cards.models import BOXES, Card

register = template.Library()

@register.inclusion_tag("cards/box_links.html")
def boxes_as_links():
    boxes = []
    for box_num in BOXES:
        card_count = Card.objects.filter(box=box_num).count()
        boxes.append({
            "number": box_num,
            "card_count": card_count,
        })

    return {"boxes": boxes}
```
This is how your custom template tag works:

- Line 737 imports Django’s template module.
- Line 739 imports your Card model and the BOXES variable.
- Line 741 creates an instance of Library used for registering your template tags.
- Line 743 uses the Library instance’s .inclusion_tag() as a decorator. This tells - Django that boxes_as_links is an inclusion tag.
- Line 746 loops over your BOXES.
- Line 747 defines card_count to keep track of the number of cards in the current box.
- Lines 748 to 751 append a dictionary with the box number as key and the number of cards in the box as the value to the boxes list.
- Line 753 returns a dictionary with your boxes data.
Hop over to navigation.html, then load your new template tag and include it:
``` html
<!-- cards/templates/navigation.html -->

{% load cards_tags %}

<nav>
    <a href="{% url 'card-list' %}">🗂 All Cards</a>
    {% boxes_as_links %}
</nav>
```
3. Pick a random card
Currently, you’re showing all your cards in a list on your box page. Enhance your BoxView to pick a random card and add it to your context:

``` python
# cards/views.py

import random

# ...

class BoxView(CardListView):
    template_name = "cards/box.html"

    def get_queryset(self):
        return Card.objects.filter(box=self.kwargs["box_num"])

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["box_number"] = self.kwargs["box_num"]
        if self.object_list:
            context["check_card"] = random.choice(self.object_list)
        return context
```
With check_card added to the context of BoxView, you can adjust box.html:

``` html
<!-- templates/cards/box.html -->

{% extends "cards/base.html" %}
{% load humanize %}

{% block content %}
    <h2>🗃 {{ box_number | ordinal }} Box</h2>
    <mark>{{ object_list | length }}</mark> Card{{ object_list | pluralize }} left in box.
    <hr>
    {% if check_card %}
        {% include "cards/card.html" with card=check_card %}
    {% endif %}
{% endblock content %}
```
Adjust card.html to take check_card into consideration:
``` html
<!-- templates/cards/card.html -->

<article>
    <h4>{{ card.question }}</h4>
    {% if not check_card %}
        <p>{{ card.answer }}</p>
        <hr>
        <a href="{% url 'card-update' card.id %}" role="button">
            ✏️ Edit Card
        </a>
    {% else %}
        <details>
            <summary>Reveal Answer</summary>
            <p>{{ card.answer }}</p>
        </details>
    {% endif %}
</article>
```

When check_card is present, you don’t show the Edit button, and you hide the answer. Otherwise, the card template shows the answer and the Edit button, just as before.

4. Check a Card
During a check session, you want to tell your flashcards app if you knew the answer or not. You’ll do this by sending a POST request that contains a form to your back end.

Create a new file named forms.py to define your Django form:
``` python
# cards/forms.py

from django import forms

class CardCheckForm(forms.Form):
    card_id = forms.IntegerField(required=True)
    solved = forms.BooleanField(required=False)
```
Before you create the form on the front end, add some code to card.html:

``` html
<!-- templates/cards/card.html -->

<article>
    <h4>{{ card.question }}</h4>
    {% if not check_card %}
        <p>{{ card.answer }}</p>
        <hr>
        <a href="{% url 'card-update' card.id %}" role="button">
            ✏️ Edit Card
        </a>
    {% else %}
        <details>
            <summary>Reveal Answer</summary>
            <p>{{ card.answer }}</p>
        </details>
        <hr>
        {% include "cards/card_check_form.html" with solved=True %}
        {% include "cards/card_check_form.html" with solved=False %}
    {% endif %}
</article>
```
To continue, create the card_check_form.html template, which contains your form. The form contains conditional formatting based on the value of the provided solved variable:

```html
<!-- cards/templates/card_check_form.html -->

<form method="post">
    {% csrf_token %}
    <input type="hidden" name="card_id" value="{{ card.id }}">
    <input type="hidden" name="solved" value="{{ solved }}">
    {% if solved %}
        <input type="submit" value="✅ I know"/>
    {% else %}
        <input type="submit" value="❌ I don't know"/>
    {% endif %}
</form>
```
Finally, you need to adjust the BoxView class to handle the POST request:

``` python
# cards/views.py

# ...

from django.shortcuts import get_object_or_404, redirect

# ...

from .forms import CardCheckForm

# ...

class BoxView(CardListView):
    template_name = "cards/box.html"
    form_class = CardCheckForm

    def get_queryset(self):
        return Card.objects.filter(box=self.kwargs["box_num"])

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["box_number"] = self.kwargs["box_num"]
        if self.object_list:
            context["card"] = random.choice(self.object_list)
        return context

    def post(self, request, *args, **kwargs):
        form = self.form_class(request.POST)
        if form.is_valid():
            card = get_object_or_404(Card, id=form.cleaned_data["card_id"])
            card.move(form.cleaned_data["solved"])

        return redirect(request.META.get("HTTP_REFERER"))
```
To check how the process works, restart your development web server and visit http://127.0.0.1:8000. Navigate to a box that contains cards, and test your knowledge

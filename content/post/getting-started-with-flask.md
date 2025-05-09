---
date: '2025-05-09T04:41:07-07:00'
draft: false
title: 'Getting Started With Flask'
tags: ["flask", "python", "web development", "beginner", "backend"]
---

## 👋 Introduction

Ever wanted to build a website but didn’t feel like wrestling with JavaScript frameworks, or felt slightly terrified by Django’s size and complexity?

Same.

Let me introduce you to **Flask**—a lightweight Python web framework that’s easy to learn, beginner-friendly, and surprisingly powerful. With just a few lines of code, you can spin up a simple web app and start building *actual stuff*.

In this post, we’ll talk about:
- What web frameworks are (and why you should care)
- What makes Flask so special
- How to get started with a simple “Hello World” app

Let’s dive in. 🐍

---

## 🤔 What Are Web Frameworks?

Flask is a *web framework*. But what even *is* a web framework?

In simple terms, a web framework is a toolbox that helps developers build websites and web apps faster and with less pain.

Sure, if you’re building a basic personal page, some HTML, CSS, and a sprinkle of JavaScript might be enough. But once you want things like:
- User accounts
- Messaging between users
- Session management
- Dynamic content

… things get complicated—fast.

A web framework handles all the annoying plumbing behind the scenes so you can focus on building features, not reinventing the wheel.

There are **frontend** frameworks (like React or Vue) and **backend** frameworks (like Flask or Django). Flask falls into the backend camp—it helps you build the part of the website users *don’t* see, but that does all the heavy lifting.

---

## 🧪 So, What Is Flask?

Flask is a Python-based micro web framework created by Armin Ronacher (legend). It was inspired by another framework called Bottle, which explains their similar minimalist vibes.

What makes Flask great?
- It’s **lightweight and unopinionated**: you choose what you want to use, nothing is forced on you.
- It’s **easy to get started with**: great for beginners.
- It’s **powerful enough for real-world apps**: many big projects started with Flask.

It also has a rich ecosystem of extensions, including:
- `Flask-Login` for user authentication
- `Flask-SQLAlchemy` for working with databases
- `Flask-Session` for session management

And the best part? If you know Python basics (functions, decorators, maybe some classes), you’re good to go.

---

## 🧰 Getting Started with Flask

Let’s create a simple Flask app that says **"Hello World"** when you open it in the browser.

### ✅ Step 1: Set up your project

First, create a new folder for your Flask app.

Make sure you have:
- Python installed on your system (and added to your PATH)
- A terminal (Command Prompt, PowerShell, Bash… pick your flavor)

## ✅ Step 1: Create and activate a virtual environment:

**Windows**
```bash
python -m venv venv
.\venv\Scripts\activate
```

**macOS/Linux**
```bash
python3 -m venv venv
source venv/bin/activate
```


## ✅Step 2: Inside the activated virtual environment, run:

```python
pip install flask
```

## ✅ Step 3: Write your first Flask app
In main.py, paste this:


```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "Hello, World!"

if __name__ == "__main__":
    app.run(debug=True)
```

Now run the file:

```python
python main.py
```
Open your browser and go to **http://localhost:5000**.

Boom. 🎉 You’ve just created your first Flask app.

## 📚 Where to Learn More

Want to go deeper? Here are some awesome resources that i also use to learn:
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Tech with Tim - Flask Tutorial](https://www.youtube.com/playlist?list=PLzMcBGfZo4-n4vJJybUVV3Un_NFS5EOgX)
- [Pretty Printed - Flask Series](https://www.youtube.com/c/PrettyPrintedTutorials)
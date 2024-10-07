---
title: "Simple Python Dependency Management"
datePublished: Mon Oct 07 2024 21:28:02 GMT+0000 (Coordinated Universal Time)
cuid: cm1zix0rc000008kw1pmlddwk
slug: simple-python-dependency-management
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728336423652/d077be02-cb7f-48f8-b50a-2492be783507.png
tags: python, python3, pip, venv, dependency-management

---

While powerful tools like virtualenv, pipenv, conda, and Poetry exist to provide extra advantages for dependency management, I’ve found a simple system that works for the small projects I’ve been working on.

This setup involves built-in features and tools that come bundled with Python by default.

When I start a new project, I create a new directory for all of its files.

```bash
mkdir my_python_project
```

Then, I change into the directory.

```bash
cd my_python_project
```

Next, I create a virtual environment.

```bash
python -m venv venv
```

This creates a new directory called `venv`. You can call it whatever you want, but I stick with this for all my projects for consistency. The new directory contains all the files and scripts necessary to manage a virtual environment.

Each session, before starting any work, I activate the virtual environment

```bash
# macOS and Linux
source venv/bin/activate

# Windows
.\venv\Scripts\activate
```

You’ll know you did it correctly when your prompt changes to have `(venv)` at the beginning. Remember, you must execute the above command each time you start a new coding session.

Installing packages works as usual with `pip install`.

```bash
pip install scikit-learn
```

But each time I use `pip install`, I freeze the dependencies into `requirements.txt`.

```bash
pip freeze > requirements.txt
```

This generates a `requirements.txt` file which lists all the required dependencies.

Any time I’m pulling in code on a new project, whether it’s my own code or someone else’s, when I see `requirements.txt`, I install the required packages with `pip`.

```bash
pip install -r requirements.txt
```

Managing dependencies this way is simple, but powerful and it works great for the kinds of projects I work on.
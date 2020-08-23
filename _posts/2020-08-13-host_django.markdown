---
layout: post
title:  "How to host Django App with Apache2 on Ubuntu 18.04"
date:   2020-08-13 15:32:50
categories: ubuntu django apache
# image: /assets/article_images/2014-08-29-welcome-to-jekyll/desktop.JPG
---
Django is a Python-based web framework. Django is great for building super fast and clean web applications. In this blog, I'll be walking you through how you can host a Django app on a Ubuntu Server.

# Basic Setup

Install the Python3-pip, Apache Server, and the Apache2-WSGI library. Apache2-WSGI library will interface with our python app and the apache2 server.

Use the command written below to install the required packages:

{% highlight shell %}
  sudo apt update
  sudo apt install python3-pip apache2 libapache2-mod-wsgi-py3
{% endhighlight %}


# Configure Python Virtual Environment

A virtual environment is created to separate python installations for different projects. You can use the following command to install the virtual environment:

{% highlight shell %}
  python3-pip install virtualenv
{% endhighlight %}

Once the virtualenv package is installed in your system, use it to create a virtual environment for your project. Command mentioned below will create a separate copy of python inside `.venv` folder, run this command inside the root directory of your Django Project.

{% highlight shell %}
  virtualenv .venv
{% endhighlight %}

Now you need to activate the newly created virtual environment, you can do this by running the following command:

{% highlight shell %}
  source .venv/bin/activate
{% endhighlight %}

Once the virtual environment is activated you'll notice a `.venv` before your shell directory. Now you need to install all the required dependencies for your project. You can do it manually or if you have a requirements file use the command below to install them all at ones:

{% highlight shell %}
  pip install -r requirements.txt
{% endhighlight %}

Now add your server's IP to the allowed hosts in your project's settings file.

{% highlight python %}
  ALLOWED_HOSTS = ["server_ip_here"]
{% endhighlight %}

Also, set the STATIC_ROOT in the settings file:

{% highlight python %}
  STATIC_URL = '/static/'
  STATIC_ROOT = os.path.join(BASE_DIR, 'static')
{% endhighlight %}

# Migrate Your Project

Once the project setup is done, migrate your project's model to the database. Use the command below to run the migration:

{% highlight shell %}
  python manage.py migrate
{% endhighlight %}

You'll also need to collect static files for CSS to load. Use the following command:

{% highlight shell %}
  python manage.py collectstatic
{% endhighlight %}

# Configure Apache

Once the project setup is done you'll be need to add Apache Site configuration. To do this move to `/etc/apache2/sites-available` and create a new configuration file:

{% highlight shell %}
  sudo touch django-project.conf
{% endhighlight %}

Now open this file to edit. You can use any text editor for this, I'll be using `nano`.

{% highlight shell %}
  sudo nano django-project.conf
{% endhighlight %}

Copy and paste the configurations below into the file

###### Change the path in the first 4 lines with your own project path.

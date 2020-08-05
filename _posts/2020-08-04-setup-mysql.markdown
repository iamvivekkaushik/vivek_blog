---
layout: post
title:  "How to Setup MySql on Ubuntu 18.04"
date:   2020-08-04 14:34:25
categories: ubuntu mysql
tags: featured
# image: /assets/article_images/2014-08-29-welcome-to-jekyll/desktop.JPG
---
MySql is an open-source database system available for linux to use for free. It's a relational database that uses SQL queries to manage its data.

{% highlight shell %}
  sudo apt update
  sudo apt install mysql-server
{% endhighlight %}

# Create new MySQL user

After installation is complete login to MySQL and create a new user. Creating a new user isn't required to manage the database but it's a nice habit to have a seprate user.

You can create a new user with the following query:

{% highlight shell %}
  CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
{% endhighlight %}

change `newuser` and `password` with your desired username and password.

# Grant all privileges

Granting all the privileges will make this user a superuser for managing any database. Do this only if you want to make the previously created user a superuser. Run the following query to grant all privilege to the user:

{% highlight shell %}
  GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
{% endhighlight %}

again, change the `newuser` with the user specified in the previous step.

# Allow Connection to this User over the internet

By default MySql doesn't allow connection outside of localhost on a MySql database. Run the following query to connect to the user from outside of localhost:


{% highlight shell %}
  Grant ALL PRIVILEGES ON *.* TO 'newuser'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
{% endhighlight %}

# Allow connection to MySQL server

Now you need to edit the mysql config file to allow connection to the MySQL server. Open the config file using the below command.

{% highlight shell %}
  cd /etc/mysql/mysql.conf.d/
  sudo nano mysqld.cnf
{% endhighlight %}

comment out the line below from the `mysqld.cnf` file with '#' symbol and save the file.

{% highlight shell %}
  bind-address = 127.0.0.1
{% endhighlight %}

# Restart MySQL Server

{% highlight shell %}
  sudo service mysql restart
{% endhighlight %}


###### Note: Make sure port '3306' is open in your security group.
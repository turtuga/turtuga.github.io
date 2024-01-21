---
layout: post
title: "Deploy Django in IIS with Fastcgi"
date: 2018-11-01 01:00:00 +0000
author: Ricardo Soares
---
To deploy Django in a IIS server, I had a quite challenges and because of this I created this post to retain my knowledge and can help someone too.

![Django]({{ site.baseurl }}/images/2021-01-30/django-logo.png "Django"){: .center-image .max-400 }

In this post I only explain how to configure the Django and install IIS, first you need to install the below packages:
- Python
- Python virtualenv (pip install virtualenv)

### Enable IIS
First you need to activate the IIS on Windows. You need to go to Control Panel -> Programs -> Turn Windows features on or off. After you came to this menu you need to activate the IIS and specially the CGI.

![IIS]({{ site.baseurl }}/images/2021-01-30/iis.png "IIS"){: .center-image }

### Place Project in IIS folder and configure wfastcgi
After the IIS instalation we need to place our Django project in IIS "wwwroot" web folder. I put my project in "C:\inetpub\wwwroot\TheProject", and now we need to create our virtualenv for our project.
- Open cmd
- Go to your project folder (cd C:\inetpub\wwwroot\TheProject)
- virtualenv venv
- cd venv\Scripts
- activate
- pip install wfastcgi
- wfastcgi-enable (if this give you an error run cmd with administration rights)

![wfastcgi]({{ site.baseurl }}/images/2021-01-30/wfastcgi.png "wfastcgi"){: .center-image }
Save the output of wfastcgi we need this to configure our project in IIS.


### Configure Information Services (IIS) Manager
Now we need to add a new website in IIS.

![newwebsite]({{ site.baseurl }}/images/2021-01-30/newwebsite.png "newwebsite"){: .center-image }

We place the site name and the project path like in the image below (in our project we choose the 8081 port), please don't click ok for now:

![newwebsitefolder]({{ site.baseurl }}/images/2021-01-30/newwebsitefolder.png "newwebsitefolder"){: .center-image }

We need to check if IIS have permissions in our project folder, click in "Test Settings..."

![permissions_error]({{ site.baseurl }}/images/2021-01-30/permissions_error.png "permissions_error"){: .center-image }

If you get an error like the image above, we need to configure your windows account to access to the project folder. Click in "Connect as..." and configure your Windows account.

![permissions_ok]({{ site.baseurl }}/images/2021-01-30/permissions_ok.png "permissions_ok"){: .center-image }

Now we can click ok and save our site settings.

### Place the web.config
Create a file called web.config in the root project folder and put the code below in the file.

![webconfig]({{ site.baseurl }}/images/2021-01-30/webconfig.png "webconfig"){: .center-image }

You can download the web.config file here [webconfig]({{ site.baseurl }}{% link /images/2021-01-30/web.config %}), please edit:
- scriptProcessor (according  to the settings in the wfastcgi-enable output)
- WSGI_HANDLER
- PYTHONPATH
- DJANGO_SETTINGS_MODULE

### Checks
Test your project with python manage.py runserver, to see if everything is ok with your Django project. If yes, navigate in your browser to http://localhost:8081/ 
If you get the error below:

![500]({{ site.baseurl }}/images/2021-01-30/500.png "500"){: .center-image }

We need change our application pool "Identity" in IIS Manager -> Application Pools -> Right click in your application pool -> Advanced Settings -> Identity  and choose LocalSystem

![identity]({{ site.baseurl }}/images/2021-01-30/identity.png "identity"){: .center-image } 

After this navigate again to http://localhost:8081/ and you should see your project

![django]({{ site.baseurl }}/images/2021-01-30/django.png "django"){: .center-image } 

Add your hostname to your Django setting’s ALLOWED_HOSTS key.


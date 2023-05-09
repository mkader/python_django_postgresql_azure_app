Azure Cloud Database for Web Apps  with Django

    Writing Django Apps
    
        Django framework : an external library & framework for server-side code. Includes an ORM for database interaction.

        Apps written in Django:
            Coursera (originally, now Scala+Play)
            Instagram
            Pinterest (originally, now Flask)
            Eventbrite

        Django web app structure - a collection of "apps" that each have their own models, views, and templates. Each app has its own folder. In this app, we have a single app called "restaurant_review": models.py, urls.py, views.py, admin.py

        Django ORM
            Define models (models.py):
                class Restaurant(models.Model):
                    name = models.CharField(max_length=50)
           
            Query models (views.py):
                def details(request, id):
                    restaurant = get_object_or_404(Restaurant, pk=id)
           
            Insert models (views.py):
                restaurant = Restaurant()
                restaurant.name = name
                Restaurant.save(restaurant)

        Configuring Django ORM  (settings.py)
            DATABASES = {
                'default': {
                    'ENGINE': 'django.db.backends.postgresql',
                    'NAME': os.environ.get('DBNAME'),
                    'HOST': os.environ.get('DBHOST'),
                    'USER': os.environ.get('DBUSER'),
                    'PASSWORD': os.environ.get('DBPASS'),
                }
            }

        Running a Django app
            python3 -m pip install -r requirements.txt
            Run DB migrations: python3 manage.py migrate
            Run the local server: python3 manage.py runserver

        Using Django admin
            Create a superuser: python manage.py createsuperuser , Login to /admin

Django apps on Azure   
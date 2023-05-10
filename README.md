Azure Cloud Database for Web Apps  with Django

    Django honeyspot is a security feature in Django, which is a Python-based web framework. The honeyspot feature is designed to protect web forms against spam and automated attacks by creating a "honeypot" field that is invisible to human users but visible to bots. Bots that try to fill out the honeypot field are considered to be spam and their submissions are rejected.

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
                    ...
                }
            }

        Check SQLTools Local Database (Make sure that .vscode/settings.json show match exactly like .env to connect local db)
        
        Running a Django app
            python3 -m pip install -r requirements.txt
            Run DB migrations: python3 manage.py migrate
            Run the local server: python3 manage.py runserver
            Create Admin account,open a new terminal: python3 manage.py createsuperuser
                Admin : https://mkader-...preview.app.github.dev/admin
    
Hosting Django apps on Azure

    Azure App Service has special code to handle Django apps. Let's use that!

    App Service + PostgreSQL inside VNet (Virtua Network) (App inside VNet with Postgres)
       
       Put both the progress server and the web app inside the virtual net, give the postgres service (called a private DNS Zone) that enables the web app to talk to it. 

       Give each of them their own subnet inside the v-net.
       
       This is roughly going to set up, this is a little complicated
 
![alt](https://raw.githubusercontent.com/mkader/python_django_postgresql_azure_app/main/app_inside_vnet.PNG) 

    Ways to deploy multiple resources on Azure
        Azure Portal (Web App + DB Template)
        Azure CLI script
        Azure Developer CLI (azd) 

        "RUN curl -fsSL https://aka.ms/install-azd.sh | bash"
        "-fsSL" are command-line options for curl:
            "-f" option means to fail silently on server errors.
            "-s" option means to mute the output and only display errors.
            "-S" option means to show errors if they occur.
            "-L" option means to follow redirects if any.
        "|" (pipe) is a cmd-line operator,redirects the o/p of the cmd on the left to the i/p of the cmd on the right.
        "bash" is a cmd-line interpreter used to execute the script that installs the Azure CLI.
    
        The cmd downloads the script that installs the Azure CLI and executes it in the Docker container during the build process.

    Azure Developer CLI (azd): This new CLI tool helps with all steps of the dev cycle:
        Step	        Command
        Provisioning	azd provision
        Deploying code	azd deploy
        ⬆️ Both	        azd up
        CI/CD	        azd pipeline config
        Monitoring	    azd monitor
    
    Components of an azd project
        infra/	    Contains all the infra in Bicep or Terraform files
        azure.yaml	Specifies what code to deploy to which resource
        .github/	Contains CI/CD workflow for deploying

        https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/make-azd-compatible?pivots=azd-convert

    azd up: First provisions, then deploys. Only re-provisions when Bicep resource definitions have changed.

    The Bicep language: is an infrastructure-as-code (IAC) language, similar to Terraform but designed for Azure. Bicep declaratively defines Azure cloud resources. Install the Bicep extension for VS Code to write bicep files.
        
    https://azure.github.io/awesome-azd/

    https://mattruma.com/cheat-sheet-azure-cli/

    http://blog.pamelafox.org/
    
    https://pamelafox.github.io/my-py-talks/pyday-databases/#/38
    
    Terminal, type:
        azd
        azd auth login
        azd up

    Only code changes, not infra, just deploy : azd deploy

    Configures the secrets for .github/workflow/azure-dev.yaml, a Github workflow that provisions and deploys: azd pipeline config

    No longer need the resources, delete them (everything in the resource group: azd down


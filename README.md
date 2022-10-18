# DjangoPostgresDocker
Setup Django with Postgres using Docker Composer

## Instalation Linux

Install Docker and Docker Compose:
```shell
sudo apt-get install docker 
sudo apt-get install docker-compose-plugin
```
Download repo using Git and move inside downloaded folder:

```shell
git https://github.com/Gieneq/DjangoPostgresDocker
cd DjangoPostgresDocker/
```
Run Docker Copmpose:

```shell
sudo docker-compose run web django-admin startproject pyrograf .
```

Change permissions:

```shell
sudo chown -R $USER:$USER pyrograf manage.py

drwxrwxr-x 5 pyrograf pyrograf 4,0K paź 18 19:54 .
drwxr-xr-x 3 pyrograf pyrograf 4,0K paź 18 19:53 ..
drwxr-xr-x 3 root     root     4,0K paź 18 19:54 data
-rw-rw-r-- 1 pyrograf pyrograf  505 paź 18 19:53 docker-compose.yml
-rw-rw-r-- 1 pyrograf pyrograf  247 paź 18 19:53 Dockerfile
-rw-rw-r-- 1 pyrograf pyrograf  186 paź 18 19:53 .dockerignore
drwxrwxr-x 8 pyrograf pyrograf 4,0K paź 18 19:53 .git
-rw-rw-r-- 1 pyrograf pyrograf   55 paź 18 19:53 .gitignore
-rwxr-xr-x 1 pyrograf pyrograf  664 paź 18 19:54 manage.py
drwxr-xr-x 2 pyrograf pyrograf 4,0K paź 18 19:54 pyrograf
-rw-rw-r-- 1 pyrograf pyrograf 1,1K paź 18 19:53 README.md
-rw-rw-r-- 1 pyrograf pyrograf   34 paź 18 19:53 requirements.txt

```

Modify pyrograf/settings.py with:

```python
# settings.py
   
import os
   
[...]
   
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('POSTGRES_NAME'),
        'USER': os.environ.get('POSTGRES_USER'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD'),
        'HOST': 'db',
        'PORT': 5432,
    }
}
```

Run Compose:

```shell
sudo docker-compose up

web_1  | Django version 4.1, using settings 'pyrograf.settings'
web_1  | Starting development server at http://0.0.0.0:8000/
web_1  | Quit the server with CONTROL-C.
```

Check additional [options](https://docs.docker.com/engine/reference/commandline/compose_up/) of _docker-compose up_. Use _--build_ to rebuild image before starting container:

```shell
sudo docker-compose up --build
```

In separate terminal check running containers:

```shell
sudo docker ps

CONTAINER ID   IMAGE                      COMMAND                  CREATED              STATUS              PORTS                                       NAMES
ca486cea36c8   djangopostgresdocker_web   "bash -c 'python man…"   About a minute ago   Up About a minute   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   djangopostgresdocker_web_1
c8fe4e3fff5b   postgres                   "docker-entrypoint.s…"   4 minutes ago        Up 4 minutes        5432/tcp                                    djangopostgresdocker_db_1

```

To shut Ctrl-C or move to project folder in separate terminal and:

```shell
sudo docker-compose down

Stopping djangopostgresdocker_web_1 ... done
Stopping djangopostgresdocker_db_1  ... done
Removing djangopostgresdocker_web_1                ... done
Removing djangopostgresdocker_web_run_71b2a772f8b8 ... done
Removing djangopostgresdocker_db_1                 ... done
Removing network djangopostgresdocker_default
```

## Interaction with running container

To use manage.py note the ID of the Django's container - in the above output it is 'ca486cea36c8'. Starting subset of the ID can be passed as parameter.

Migrate:

```shell
sudo docker exec -it ca48 python manage.py migrate
```

Superuser:

```shell
sudo docker exec -it ca48 python manage.py createsuperuser
```

Load fixtures:

```shell
sudo docker exec -it ca48 python manage.py loaddata fixtures.json
```

Tests:

```shell
sudo docker exec -it ca48 python manage.py test
```

## Reference

- [Docker Django PostgresSQL](https://docs.docker.com/samples/django/)


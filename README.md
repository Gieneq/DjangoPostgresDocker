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
sudo chown -R $USER:$USER composeexample manage.py
```

Modify pyrograf/settings.py
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

```shell
docker-compose up
docker ps
docker-compose down
```


## Reference

- [Docker Django PostgresSQL](https://docs.docker.com/samples/django/)








```shell
docker-compose up --build
```
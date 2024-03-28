# machine learning leaderboard

Full-stack todo application with authentication. Built with Django, MySQL, Postgres and NGINX.

App should be accessible through : 

- http://localhost:80 (NGINX Proxy for DJANGO SERVER)
- http://localhost:8000 (DJANGO SERVER)

Create a microservices architecture through docker-compose using 4 services

- client (code in ./leaderboard)
- api (code in ./server)
- database1 (use postgres)
- database2 (use mysql)
- proxy (use nginx)

We want to be able to choose which database to use, and change databases on different deployments, for this we can use an ENV variable to set which database host to use when launching the containers. You will have to modify the `leaderboard/leaderboard/settings.py` file in order to accommodate the automatic changes in databases. (The code is now running sqlite3 for ease of use)

You must configure the services, ports, and database information for the app to work correctly. Read the code to see what ports are used. NGINX should be on port `80`. The default port for POSTGRES is `5432`. The default port for MySQL is `3306`.

You will have to write a good NGINX config file.

In order to test uploaded models, this leaderboard can launch other docker images using the Processor module. We have two processor available for usage, one for pytorch and another for keras.

You must create database entries for each of them in Processors, in the django admin interface using : 


```
# Entry 1
# Image name : fennecinspace/hackia-keras-fire-classify
# Command 
docker run -v Resource.name[FIRE_CLASSIFICATION_TEST_DATASET]:/code/dataset -v Submission.file:/code/model.h5 fennecinspace/hackia-keras-fire-classify python /code/script.py -d /code/dataset -m /code/model.h5

# Entry 2
# Image name : fennecinspace/hackia-torch-fire-classify
# Command 
docker run -v Resource.name[FIRE_CLASSIFICATION_TEST_DATASET]:/code/dataset -v Submission.file:/code/model.pt fennecinspace/hackia-torch-fire-classify python /code/script.py -d /code/dataset -m /code/model.pt
```

The docker images used by the processors already exist on docker hub, no need to dockerize that code, it will work automatically if the entries are created correctly and the databases are configured correctly as well.

A database instance should be uploaded to the leaderboard for testing, to do that create a Resource database entry in django admin and upload the contents of `leaderboard/test` zipped (test.zip), for the name attribute use `FIRE_CLASSIFICATION_TEST_DATASET`

Make sure to configure all static and media file paths correctly for NGINX proxy.
<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="200" alt="Laravel Logo"></a><a href="https://laravel.com" target="_blank"><img src="https://www.google.com/imgres?imgurl=https%3A%2F%2Fdomain-forward.com%2Fwp-content%2Fuploads%2F2023%2F07%2Fgoogle-cloud-run-logo.webp&tbnid=S2ITlWCF0FrpKM&vet=12ahUKEwj95IPlp_uDAxUwWEEAHTOCAAEQMygBegQIARBP..i&imgrefurl=https%3A%2F%2Fdomain-forward.com%2F2023%2F07%2F13%2Fredirect-traffic-to-another-domain-using-google-cloud-run%2F&docid=4t_WDE58_piFNM&w=769&h=411&q=gcp%20cloud%20run%20logo&ved=2ahUKEwj95IPlp_uDAxUwWEEAHTOCAAEQMygBegQIARBP" width="200" alt="Laravel Logo"></a></p>



## About this repository

You can use docker file and google cloud run to deploy the laravel application. 


- [Google Cloud Run ](https://console.cloud.google.com/run).
- [DockerFile ](https://docs.docker.com/engine/reference/builder/).
- Cloud SQL [Cloud SQL ](https://cloud.google.com/sql/docs/introduction) 

Laravel will be deployed on serverless Cloud Run using docker file and startup scripts. 

## Tutorial 

1. Run a local php server including all the dependencies for php version and composer.
2. Add the Dockerfile to it. 

```
FROM php:8.2-fpm-alpine #change the php version accordingly

RUN apk add --no-cache nginx supervisor wget #install other packages if require like pdo_psql

RUN mkdir -p /run/nginx #this creates a nginx folder

COPY docker/nginx.conf /etc/nginx/nginx.conf #it copies the nginx conf from docker folder as mentioned in repo.

RUN mkdir -p /app
COPY . /app 

#this install the composer dependencies
RUN sh -c "wget http://getcomposer.org/composer.phar && chmod a+x composer.phar && mv composer.phar /usr/local/bin/composer"
RUN cd /app && \
    /usr/local/bin/composer install --no-dev 

#if it gives platform errors try running with flaf --ignore-platform-reqs

RUN chown -R www-data: /app

CMD sh /app/docker/startup.sh
```


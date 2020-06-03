---
id: 
title: Nginx location priority for React-Router and Drupal backend
date: 2020-05-17
author: Clayton Salem
categories:
    - server
    - front end
    - backend
---

## Goal
Create a front-end single page application, completely seperate from a Drupal backend, and yet hosted at the same url as the backend  admin interface. In addition, the client required separate linkable urls for the front-end application.

[React-Router](https://reacttraining.com/react-router/) can take care of the front-end routing issues from root, however, we need to be able to land on `example.com/path/to/subpage`, and have it load the html page at the root of the front-end application. This is in addition to being able to access the drupal site at `example.com/drupal/user/login` as well as fetching data at `example.com/drupal/jsonapi/node/page`.

In other words:

```
/ --> HTML
/page/to --> HTML
/page/to/subpage --> HTML
/drupal/user/login --> Drupal site
/drupal/jsonapi/node/page --> Drupal site

```
In order to solve this with "clean" urls, I turned to nginx configuration.


## Nested application with location blocks

The starting point came from a co-worker who pointed me to this video on [nesting php applications](https://serversforhackers.com/c/nginx-php-in-subdirectory). Location blocks allow us to tell nginx to process different urls in different manners. In this case, we want to tell it that any traffic to `/drupal` should be directed to the subdirectory in which the Drupal site is installed. 

#### Simple is eligent:
The journey into nginx configuration brought me to some interesting places, including location block priority, negative look-aheads in regex, and others. In the end, the simplest configuration won out.

```
root /var/www/example/frontend;

location / {
    try_files $uri $uri/ /index.html;
}

location /drupal {
    alias /var/www/example/web/;
    try_files $uri $uri/ @rewrite;
}

location @rewrite {
    rewrite /drupal/(.*)$ /drupal/index.php? last;
}
```

Let's go through line by line.
- Since the html page serves as the primary site, we set that locaiton as `root`.
- Traffic to the root of the site `/` will be served from the root. First, we check for the file requested(`$uri`), then check if that's a directory (`$uri/`), finally, we'll generate the response from the index file `/index.html`.
- Any traffic to `/drupal` is aliased to the Drupal installation in the `web` directory.
- Much like traffic to the root, we check files and directories, but then we pass the request to the `@rewrite`.
- the rewrite passes any part of the path after `/drupal/` as a query parameter to Drupal's primary index file. We also tack a `last` on the end to let nginx know the rewrite is finished.


The html page is at `/var/www/example/fronend`, the drupal site at `/var/www/example/web`. The alias aligns any request to `/drupal` to the directory `web`. The rewrite passes the rest of the request uri to Drupal's index file as a query parameter.


## JsonAPI throws an error
The previous configuration worked for a first pass. Any traffic to `example.com` results in the html page and the React app, and any traffic to `example.com/drupal` results in the drupal site loading. However, when I tried to fetch data from the React application, I was getting an error from the Drupal JSONAPI. 

## Location Priority
https://docs.nginx.com/nginx/admin-guide/web-server/web-server/#nginx-location-priority

examples:

```
location / {

}

location ~ / {

}
```

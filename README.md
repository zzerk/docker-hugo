zzerk/hugo
==============

`zzerk/hugo` is a [Docker](https://www.docker.io) base image for static sites generated with [Hugo](http://gohugo.io).  

This image is uses the `extended` release of Hugo, which contains support for Sass/SCSS.

Images derived from this image can either run as a stand-alone server, or function as a volume image for your web server.  You can also use them in a CI/CD system such as Gitlab CI to build your content prior to bundling it into a standalone webserver container such as `httpd:alpine`.

It is heavily based on `monachus/hugo` and tweaked for my personal usage by adding following packages:
- lftp for deployment using FTP
- png/jpeg optimisation (optipng and jpegoptim)


Prerequisites
-------------

The image is based on the following directory structure:

	.
	├── Dockerfile
	└── site
	    ├── config.toml
	    ├── content
	    │   └── ...
	    ├── layouts
	    │   └── ...
	    └── static
		└── ...

In other words, your Hugo site resides in the `site` directory, and you have a simple Dockerfile:

	FROM zzerk/hugo 


Building your site
------------------

Based on this structure, you can easily build an image for your site:

	docker build -t zzerk/hugo .

Your site is automatically generated during this build. 


Using your site
---------------

There are two options for using the image you generated: 

- as a stand-alone image
- as a volume image for your webserver

Using your image as a stand-alone image is the easiest:

	docker run -p 1313:1313 zzerk/hugo

This will automatically start `hugo server`, and your blog is now available on http://localhost:1313. 

If you are using `boot2docker`, you need to adjust the base URL: 

	docker run -p 1313:1313 -e HUGO_BASE_URL=http://YOUR_DOCKER_IP:1313 zzerk/hugo

The image is also suitable for use as a volume image for a web server, such as [nginx](https://registry.hub.docker.com/_/nginx/)

	docker run -d -v /usr/share/nginx/html --name site-data zzerk/hugo
	docker run -d --volumes-from site-data --name site-server -p 80:80 nginx


Usage with Gitlab CI/CD
-----------------------
The image can be use to automate web site generation and deployment using Gitlab CI/CD. 
At the beginning of your `.gitlab-ci.yml` file, just specify:

	image: zzerk/hugo


Examples
--------

For an example of a Hugo site, have a look at <https://eric.bustarret.com>

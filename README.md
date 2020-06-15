# Heroku Buildpack Nginx

This buildpack will work out-of-the-box with nginx. 
By default it will serve the website from the `html` directory. 
You can easily overwrite that in the custom nginx config file or with the heroku config variables.

## Usage

Creating a new Heroku instance from the application's parent directory:

    $ heroku create --buildpack https://github.com/eccegordo/heroku-buildpack-nginx.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    ...

Or add buildback to existing Heroku app

    $ heroku config:add BUILDPACK_URL=https://github.com/eccegordo/heroku-buildpack-nginx.git

## Configuration

### Variables

You can set a few different environment variables to turn on features in this buildpack.

#### Root directory

Set the root directory with you website (Default: `html`):

    heroku config:set ROOT_DIRECTORY=html

#### Nginx Workers

Set the number of workers for Nginx (Default: `4`):

    heroku config:set NGINX_WORKERS=4

#### APP Service

Set the location of the APP service/server you want to proxy to.
`/app => ...`

    heroku config:set APP_SERVER=https://app-example.herokuapp.com
    heroku config:set APP_SERVER_PATH=/app
    heroku config:set APP_SERVER_SUBDOMAIN=app-example


#### AUTH Service

Set the location of the auth service you want to proxy to.
`/auth => ...`

    heroku config:set AUTH_SERVER=https://auth-example.herokuapp.com
    heroku config:set AUTH_SERVER_PATH=/auth
    heroku config:set AUTH_SERVER_SUBDOMAIN=auth-example

#### API Service

Set the location of the API service you want to proxy to. 
`/api => ...`

    heroku config:set API_SERVER=https://api-example.herokuapp.com
    heroku config:set API_SERVER_PATH=/api
    heroku config:set API_SERVER_SUBDOMAIN=api-example

#### Authentication

Have a staging server? Want to protect it with authentication? When `BASIC_AUTH_USER` and `BASIC_AUTH_PASSWORD` are set basic authentication will be activated:

    heroku config:set BASIC_AUTH_USER=EXAMPLE_USER
    heroku config:set BASIC_AUTH_PASSWORD=EXAMPLE_PASSWORD

*Be sure to use `https` when you set this up for added security.*

#### Force HTTPS/SSL

For most Ember applications that make any kind of authenticated requests (sending an auth token with a request for example), HTTPS should be used. Enable this feature in nginx by setting `FORCE_HTTPS`.

    heroku config:set FORCE_HTTPS=true

#### Redirect not found to

Have one index.html file and you want to redirect everything not found to that single file?

    heroku config:set REDIRECT_NOT_FOUND_TO=/index.html

#### Before and After Hooks

You can run your own scripts by creating `after_hook.sh` or `before_hook.sh` files (or both) in your app's `hooks` directory:

    mkdir hooks
    cd hooks
    touch after_hook.sh
    touch before_hook.sh

### Custom Nginx

Need to make a custom nginx configuration change? No problem. In your application, add a `config/nginx.conf.erb` file. You can copy the existing configuration file in this repo and make your changes to it.

### Useful documentation

https://devcenter.heroku.com/articles/buildpack-api

# PwnAuth

A web application framework for launching and managing OAuth abuse campaigns.

Created By Doug Bienstock [(@doughsec)](https://twitter.com/doughsec) while at Mandiant/FireEye
## Minimum requirements

* An Internet accessible server (tested running Ubuntu 16.04)
* Nginx
* Docker (apt install docker.io)
* Docker-Compose (newest version from docker website)
* A Valid SSL certificate

## Installation


1. Clone the repository onto your server
2. Inside `Dockerfile` customize the settings to your site:
    * Change `DJANGO_SITE` to match the FQDN or IP address you will use to access the PwnAuth App.
    * Change the `SECRET_KEY` to a new random value.
    * Set `DJANGO_ENV` to `prod` or `dev` depending on if you are in production or not.
3. Configure your SSL certificates and NGINX. I have provided a sample NGINX configuration in `nginx/oauth.conf`
4. Run `setup.sh` as root. This will build the docker services for the OAuth application as well as setup an initial Django administrator for you to use the application with.
5. Login to the app. Navigate to `/auth/login` in your browser to login to the application with the account you just created.

For more first use instructions see [the wiki](https://github.com/fireeye/PwnAuth/wiki)
## Modules

PwnAuth is designed to be modular. A new Identity Provider can easily be supported by developing the necessary database models and views to interact with the Resource Server.
As long as you follow the module implementation guidelines, the GUI will automatically detect the module and it will be ready for use.

### Office 365

1. You must create a new OAuth application with microsoft at the [Microsoft App Portal](https://apps.dev.microsoft.com)
2. You must create a "Web" platform with a proper Redirect URL. The default configuration for PwnAuth is `/oauth/api/microsoft/callback`
3. Be sure to create a secret key and ensure your scopes include `user.read` and `offline_access`
4. Import the application settings into the application using the GUI
5. Send out your phishing emails using the `authorization_url_full` link and wait for responses!

## Usage

PwnAuth is designed to be interacted with inside of a browser. There is also an API available available for power users. To learn more about using PwnAuth see [the wiki](https://github.com/fireeye/PwnAuth/wiki).

## Logging

Logs are written to `/var/log/oauth` on your host system where docker is running. The `audit` log records all actions taken in the application and by whom.

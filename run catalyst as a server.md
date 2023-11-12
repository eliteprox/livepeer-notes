# Running Catalyst Box as Server

Since livepeer-in-a-box is initially configured to work only from `localhost`, the following notes describe how to configure it successfully configure it to be deployed to a server. 
1. Create a development folder  `mkdir ~\catalyt-dev`
2. Follow the steps outlined here: https://catalyst-docs.iame.li/catalyst/developing-with-catalyst
3. Follow these steps to create a self-signed certificate and reverse proxy to http port 8888. https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04#step-2-configure-nginx-to-use-ssl
Example HTTPS proxy configuration `/etc/nginx/sites-available/default`
```
server {

    # SSL configuration

    listen 443 ssl http2 default_server;
    listen [::]:443 ssl http2 default_server;
    include snippets/self-signed.conf;
    include snippets/ssl-params.conf;

    location /
    {
          proxy_pass "http://127.0.0.1:8888";
          proxy_http_version 1.1;
    }

}
```

4. Open `catalyst-dev/catalyst/config/full-stack.json` in Visual Studio Code editor. 
5. Under the `livepeer-api` connector, modify the following settings:
   -  Add your server's IP or domain to `cors-jwt-allowlist`, example: `\"https://192.168.10.61\"`. Be mindful of the escape characters, they are required.
   -  Modify `ingest`, change every instance of `localhost` to your server domain or IP. Change `http` to `https` also and remove `:8888` where found. 
6.  Under the `livepeer-catalyst-api` connector, modify the following settings:
   -  Modify `tags` from `http://localhost:8888` to `https://serveripordomain`
7. Stop the docker instance and re-run `make box-dev` to reload livepeer api with the new configuration.

Known issue: 
- The preview player and share button link point to https://lvpr.tv?v=29282syji2nx5l0z, but the streams are not available there of course. This will require a slight change within livepeer studio.
- TBD as testing is completed on other features

## TODO:  Configuring livepeer-in-a-box to connect to the livepeer network! (Real orchestrators)



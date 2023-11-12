# Running Catalyst Box as Server

Since livepeer-in-a-box is initially configured to work only from `localhost`, the following notes describe how to configure it successfully configure it to be deployed to a server. 

In summary, 



1. Create a development folder where we can develop the various components of catalyst

`mkdir ~\catalyt-dev`

2. Follow the steps outlined here: https://catalyst-docs.iame.li/catalyst/developing-with-catalyst
3. Follow these steps to create a self-signed certificate and reverse proxy to http port 8888. https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04#step-2-configure-nginx-to-use-ssl
4. Open `catalyst-dev/catalyst/config/full-stack.json` in Visual Studio Code editor. 
5. Under the `livepeer-api` connector, modify the following settings:
   -  Add your server's IP or domain to `cors-jwt-allowlist`, example: `\"https://192.168.10.61\"`. Be mindful of the escape characters, they are required.
   -  Modify `ingest`, change every instance of `localhost` to your server domain or IP. Change `http` to `https` also. 
6.  Under the `livepeer-catalyst-api` connector, modify the following settings:
   -  Modify `tags` from `http://localhost` to `https://serveripordomain`

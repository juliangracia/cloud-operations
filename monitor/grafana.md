# Grafana
The analytics platform for all your metrics.
Grafana allows you to query, visualize, alert on and understand your metrics no matter where they are stored. Create, explore, and share dashboards with your team and foster a data driven culture.

## Install Grafana on a GCP Compute Engine.
### Before you begin
1. Select or create a GCP Project
    > Note: If you don't plan to keep the resources that you create in this procedure, create a project instead of selecting an existing project. After you finish these steps, you can delete the project, removing all resources associated with the project.
1. Make sure that billing is enabled for your Google Cloud Platform project.

### Prerequisites
1. Your Compute Engine Instance running.
1. For setting up Compute Engine, see the [Setting up Compute Engine Instance](https://www.cloudbooklet.com/setting-up-google-cloud-compute-engine-instance/).
1. Connect to the newly created install with SSH connection button.

### Step 1: Update Server
Make sure your server is upto date.
```
sudo apt update
sudo apt upgrade
```
### Step 2: Install Grafana APT Repository
The command `add-apt-repository` isn’t a default app on Debian 9 and requires
```
sudo apt-get install -y software-properties-common
```
Install the repository for stable releases
```
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```
Then add our gpg key. This allows you to install signed packages.
```
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```
On some older versions of Ubuntu and Debian you may need to install the apt-transport-https package which is needed to fetch packages over HTTPS.
```
sudo apt-get install -y apt-transport-https
```
Update your Apt repositories and install Grafana
```
sudo apt-get update
sudo apt-get install grafana
```
Start Grafana.
```
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```
You can check the status of Grafana using this command
```
sudo systemctl status grafana-server
```
You will receive an output similar to this.
```
● grafana-server.service - Grafana instance
    Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; enabled; vendor preset: enabled)
    Active: active (running) since Tue 2019-07-30 09:36:47 UTC; 15s ago
      Docs: http://docs.grafana.org
  Main PID: 4399 (grafana-server)
     Tasks: 10 (limit: 667)
    CGroup: /system.slice/grafana-server.service
            └─4399 /usr/sbin/grafana-server --config=/etc/grafana/grafana.ini
```
Enable it to startup on boot.
```
sudo systemctl enable grafana-server
```
### Step 3: Install Nginx
Once Grafana is installed you can install Nginx and configure it.
```
sudo apt install nginx
```
### Step 4: Configure Nginx
Remove the default Nginx configuration.
```
sudo rm -rf /etc/nginx/sites-available/default
sudo rm -rf /etc/nginx/sites-enabled/default
```
Create a new configuration with the reverse proxy setup for Grafana
```
sudo nano /etc/nginx/sites-available/grafana.conf
```
Paste the following in the file.
```
server {
     listen 80;

     server_name yourdomainname.com;

     location / {
         proxy_pass http://localhost:3000;
         proxy_http_version 1.1;
         proxy_set_header Upgrade $http_upgrade;
         proxy_set_header Connection 'upgrade';
         proxy_set_header Host $host;
         proxy_cache_bypass $http_upgrade;
     }
}
```
Hit `Ctrl + X` followed by `Y` and `Enter` to save and exit the file.

Enable the configuration by creating a symbolic link to `sites-enabled` directory.
```
sudo ln -s /etc/nginx/sites-available/grafana.conf /etc/nginx/sites-enabled/grafana.conf
```

Restart Nginx
```
sudo nginx -t
sudo service nginx restart
```

### Step 5: Check the Installation
- Now check your domain name on your browser.
- You will see the login page of Grafana.
- The default username is `admin` and the password is `admin`
- In the next screen you can setup a new password.
- Once you setup a new password you will land at the Grafana dashboard.


### Clean Up
- Delete the VM instance
---
---
## Google OAuth2 Authentication
To enable the Google OAuth2 you must register your application with Google. Google will generate a client ID and secret key for you to use.
### Create Google OAuth keys

First, you need to create a Google OAuth Client:

1.  Go to [https://console.developers.google.com/apis/credentials](https://console.developers.google.com/apis/credentials)
2.  Click the ‘Create Credentials’ button, then click ‘OAuth Client ID’ in the menu that drops down
3.  Enter the following:
    *   Application Type: Web Application
    *   Name: Grafana
    *   Authorized Javascript Origins: [https://grafana.mycompany.com](https://grafana.mycompany.com)
    *   Authorized Redirect URLs: [https://grafana.mycompany.com/login/google](https://grafana.mycompany.com/login/google)
    *   Replace [https://grafana.mycompany.com](https://grafana.mycompany.com) with the URL of your Grafana instance.
4.  Click Create
5.  Copy the Client ID and Client Secret from the ‘OAuth Client’ modal

### Enable Google OAuth in Grafana

Specify the Client ID and Secret in the [Grafana configuration file](https://grafana.com/docs/installation/configuration/#config-file-locations). For example:
```
    [auth.google]
    enabled = true
    client_id = CLIENT_ID
    client_secret = CLIENT_SECRET
    scopes = https://www.googleapis.com/auth/userinfo.profile https://www.googleapis.com/auth/userinfo.email
    auth_url = https://accounts.google.com/o/oauth2/auth
    token_url = https://accounts.google.com/o/oauth2/token
    allowed_domains = mycompany.com mycompany.org
    allow_sign_up = true
```    

You may have to set the `root_url` option of `[server]` for the callback URL to be correct. For example in case you are serving Grafana behind a proxy.

Restart the Grafana back-end. You should now see a Google login button on the login page. You can now login or sign up with your Google accounts. The `allowed_domains` option is optional, and domains were separated by space.

You may allow users to sign-up via Google authentication by setting the `allow_sign_up` option to `true`. When this option is set to `true`, any user successfully authenticating via Google authentication will be automatically signed up.
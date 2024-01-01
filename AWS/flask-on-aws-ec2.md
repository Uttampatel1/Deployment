# Run Flask App on AWS EC2 Instance 🚀

## Install Python Virtualenv 🐍

```bash
sudo apt-get update
sudo apt-get install python3-venv
```
## Clone Flask App from GitHub or Use a Zip File 📦

### Clone from GitHub

```bash
git clone https://github.com/your-username/your-flask-app.git
cd your-flask-app
```
or
### Download and Extract Zip File

```bash
wget https://github.com/your-username/your-flask-app/archive/main.zip
unzip main.zip
cd your-flask-app-main
```
or
### new directory 📂

```bash
mkdir helloworld
cd helloworld
```


## Activate the new virtual environment 

## Create the virtual environment 🛠️

```bash
python3 -m venv venv
```

## Activate the virtual environment 🚀

```bash
source venv/bin/activate
```

## Install Flask 🌐

```bash
pip install -r requirement.txt
```


## Verify if it works by running ▶️

```bash
python app.py
```

## Run Gunicorn WSGI server to serve the Flask Application 🦄

Install Gunicorn using the below command:

```bash
pip install gunicorn
```

Run Gunicorn:

```bash
gunicorn -b 0.0.0.0:8000 app:app 
```

Gunicorn is running (Ctrl + C to exit gunicorn)!

## Use systemd to manage Gunicorn 🚁

Systemd is a boot manager for Linux. We are using it to restart gunicorn if the EC2 restarts or reboots for some reason.

Create a `helloworld.service` file in the `/etc/systemd/system` folder.

```bash
sudo nano /etc/systemd/system/helloworld.service
```

Add the following into the file.

```ini
[Unit]
Description=Gunicorn instance for a simple hello world app
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/helloworld
ExecStart=/home/ubuntu/helloworld/venv/bin/gunicorn -b localhost:8000 app:app
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl start helloworld
sudo systemctl enable helloworld
```

Check if the app is running with:

```bash
curl localhost:8000
```

## Run Nginx Webserver to accept and route requests to Gunicorn 🌐

Finally, set up Nginx as a reverse-proxy to accept requests from the user and route them to Gunicorn.

Install Nginx:

```bash
sudo apt-get install nginx
```

Start the Nginx service and go to the Public IP address of your EC2 on the browser to see the default nginx landing page:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Edit the default file (below the default comments):

```bash
sudo nano /etc/nginx/sites-available/default
```

Add the following code at the top of the file in the `sites-available` folder.

```nginx
upstream flaskhelloworld {
    server 127.0.0.1:8000;
}
```

Add a proxy_pass to `flaskhelloworld` at location `/`:

```nginx
location / {
    proxy_pass http://flaskhelloworld;
}
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```


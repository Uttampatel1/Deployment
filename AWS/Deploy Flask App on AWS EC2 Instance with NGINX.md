# Deploy Flask App on AWS EC2 Instance with NGINX 🚀

## Step 1: SSH into your EC2 instance

```bash
ssh -i /path/to/your/key.pem ec2-user@your-ec2-ip-address
```

Ensure you have the necessary permissions to access the EC2 instance.

## Step 2: Install Python and Git 🐍🚀

```bash
sudo yum update -y
sudo yum install python3 git -y
```

Ensure you have the latest version of Python installed and Git for version control.

## Step 3: Clone your Flask app from GitHub 📂

```bash
git clone https://github.com/your-username/your-flask-app.git
cd your-flask-app
```

Replace `your-username` and `your-flask-app` with your GitHub username and the name of your Flask app repository.

## Step 4: Create a virtual environment and activate it 🛠️

```bash
python3 -m venv venv
source venv/bin/activate
```

Set up a virtual environment to isolate your project dependencies.

## Step 5: Install Gunicorn 🦄

```bash
pip install gunicorn
```

Gunicorn is a production-ready WSGI server that will serve your Flask application.

## Step 6: Install dependencies from requirements.txt file 📦

```bash
pip install -r requirements.txt
```

Ensure all required Python packages are installed based on your project's dependencies.

## Step 7: Try running the app through Gunicorn ▶️

```bash
gunicorn -b 0.0.0.0:8000 app:app
```

Test if the app is running properly with Gunicorn.

## Step 8: Create a systemd service file for your app ⚙️

Create a service file `/etc/systemd/system/yourapp.service`:

```ini
[Unit]
Description=Gunicorn instance for a simple hello world app
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/your-flask-app
ExecStart=/home/ubuntu/your-flask-app/venv/bin/gunicorn -b localhost:8000 app:app
Restart=always

[Install]
WantedBy=multi-user.target
```

Customize this file based on your app's specifics.

## Step 9: Enable the service ✔️

```bash
sudo systemctl daemon-reload
sudo systemctl start yourapp
sudo systemctl enable yourapp
```

Enable and start the systemd service for your Flask app.

## Step 10: Check if the app is running with 🌐

```bash
curl localhost:8000
```

Ensure that your app is accessible and running.

## Step 11: Install NGINX 🌐

```bash
sudo yum install nginx -y
```

Install NGINX to act as a reverse proxy for your Flask app.

## Step 12: Edit the NGINX default file ✏️

```bash
sudo nano /etc/nginx/sites-available/default
```

Edit the default NGINX configuration file.

## Step 13: Add the following code at the top of the file 🚦

```nginx
upstream flaskapp {
    server 127.0.0.1:8000;
}

server {
    listen 80;
    server_name your-domain-or-ip;

    location / {
        proxy_pass http://flaskapp;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /static {
        alias /home/ubuntu/your-flask-app/static;
    }

    location /templates {
        alias /home/ubuntu/your-flask-app/templates;
    }

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Customize the NGINX configuration based on your app's structure.

## Step 14: Restart NGINX 🔄

```bash
sudo systemctl restart nginx
```

Restart NGINX to apply the changes.


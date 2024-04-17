# Setting Up Server Environment and Python on CentOS

## Adding Users

Throughout this tutorial, we will be working with the user `sammy`. Please substitute with the username of your choice.

You can add a new user by typing:
```
sudo adduser sammy
```

Next, give your user a password using the `passwd` command:
```
sudo passwd Sammy
```

Now your new user is set up and ready for use!

## Granting Sudo Privileges to a User

If your new user should have the ability to execute commands with root (administrative) privileges, you will need to give the new user access to sudo.

We can do this by adding the user to the `wheel` group (which gives sudo access to all of its members by default).

To do this, use the `usermod` command:
```
sudo usermod -aG wheel sammy
```

Now your new user is able to execute commands with administrative privileges. To do so, simply type `sudo` ahead of the command that you want to execute as an administrator:
```
sudo some_command
```

## Installing Python 3.10 on CentOS

### Step 1: Update CentOS

Update your system with the following command:
```
yum update
```

### Step 2: Install necessary packages

Next, install some packages:
```
yum install gcc git zlib-devel openssl11-devel libffi-devel bzip2-devel ncurses-devel readline-devel xz-devel sqlite-devel
```
OR
```
yum groupinstall "Development Tools"
```
OR
```
yum install devtoolset-9
scl enable devtoolset-9 bash
```

### Step 3: Download Python

Download Python 3.10.2:
```
wget https://www.python.org/ftp/python/3.10.2/Python-3.10.2.tgz
```

Extract the archive:
```
tar -xzf Python-3.10.2.tgz
```

### Step 4: Install Python 3.10

To install Python 3.10.2, navigate to the directory:
```
cd Python-3.10.2
```

Run the following command:
```
./configure --enable-optimizations
```

Compile Python:
```
make altinstall
```

Verify the installation:
```
python3.10 -V
```

## Install Components from Repositories

Our first step will be to install all of the pieces that we need from the repositories. We will need to add the EPEL repository, which contains some extra packages, in order to install some of the components we need.

You can enable the EPEL repo by typing:
```
sudo yum install epel-release
```

Once access to the EPEL repository is configured on our system, we can begin installing the packages we need. We will install pip, the Python package manager, in order to install and manage our Python components. We will also get a compiler and the Python development files needed to build uWSGI. We’ll install Nginx now as well.

You can install all of these components by typing:
```
sudo yum install python-pip python-devel gcc nginx
```

## Create a Python Virtual Environment

Start by installing the virtualenv package using pip:
```
sudo pip install virtualenv
```

Now, make a parent directory for our Flask project:
```
mkdir ~/myproject
cd ~/myproject
```

Create a virtual environment:
```
virtualenv myprojectenv
```
OR
```
virtualenv venv --python=python2.7
```

Activate the virtual environment:
```
source myprojectenv/bin/activate
```

## Set Up a Flask Application

Now that you are in your virtual environment, install Flask and uWSGI:
```
pip install flask
```

### Create a Sample App

Create a file `myproject.py`:
```
nano ~/myproject/myproject.py
```

Add the following code to `myproject.py`:
```python
from flask import Flask
application = Flask(__name__)

@application.route("/")
def hello():
    return "<h1 style='color:blue'>Hello There!</h1>"

if __name__ == "__main__":
    application.run(host='0.0.0.0')
```

Test your Flask app by typing:
```
python myproject.py
```

Visit your server’s domain name or IP address followed by the port number specified in the terminal output (most likely :5000) in your web browser. 

When you are finished, hit `CTRL-C` in your terminal window a few times to stop the Flask development server.

## Troubleshooting

### If Error: ModuleNotFound error: No module named '_ssl'

Refer: [Issue on GitHub](https://github.com/pyenv/pyenv/issues/2760)

### ImportError: urllib3 v2 only supports OpenSSL 1.1.1+

Refer: [Discussion on GitHub](https://github.com/explosion/spaCy/discussions/12750)

### Run Flask App Error --- AssertionError: View function mapping is overwriting an existing endpoint function: main

Refer: [Article on TecAdmin](https://tecadmin.net/assertionerror-view-function-mapping-is-overwriting-an-existing-endpoint-function/)

## Useful Links

- [Initial Server Setup with CentOS](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos)
- [Add and Delete Users on a CentOS 7 Server](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-a-centos-7-server)
- [Open/Close Ports CentOS 6/7](https://www.basezap.com/open-close-ports-centos-6-7/)
- [How to Install Python on CentOS](https://zomro.com/blog/faq/294-kak-ustanovit-python-310-na-centos-7)
- [How to Check RAM Size in CentOS/RedHat](https://kb.qualispace.com/knowledge-base/how-to-check-ram-size-in-centos-redhat/)
- [How to Disable/Stop Firewall CentOS](https://phoenixnap.com/kb/how-to-disable-stop-firewall-centos)
- [Disable Firewall CentOS](https://monovm.com/blog/centos-disable-firewall/)



# PyFoundry
**PyFoundry** is a Docker-based environment for building **standalone Python environments** for system transplantation.

It is useful in the folowing scenarios:
* Transplantation to air gapped systems with no internet connectivity.
* Systems where you want a clean pip driven Python without system Python packages (Ubuntu?),
* Instances where you don't want to disturb a Python environment on a production project, or have tainted libraries that you don't want to disturb. Simple package installs can trigger updates that will brake things.
* Would like a newer or older working version of Python than your operating system distribution might support out of the box.

PyFoundry uses [pyenv](https://github.com/pyenv/pyenv) together with [virtualenv](https://virtualenv.pypa.io/en/latest/) to manage isolated Python builds.  
Docker serves as the **build platform**.

# Getting started and working with the system
## Getting Started

### 1 Build the Docker Container
Build a container for the platform you’re targeting.  
For example, to build a Python environment for **Ubuntu 18.04**:
```bash
cd ubuntu18.04
./compile
```
That will build the container, and enter it. From there you can start generating Python environments.

### 2 Create a New Environment
Example: build Python 3.6.15 for a project called test. While within the docker container for your chosen platform:
```bash
newenv 3.6.15 test
```
This downloads, compiles, and installs:

The base Python environment → /python_virtualenvs/versions/3.6.15

A project environment → /python_virtualenvs/test/

### 3 Install Packages
To install the paramiko module using pip :
```bash
cd /python_virtualenvs/test/bin
./pip install --only-binary=:all: paramiko
```
The --only-binary=:all: flag forces binary wheels, avoiding painful source builds that usually brake.


### 4 Test the Environment
```bash
cd /python_virtualenvs/test/bin
./python
>>> import paramiko
>>> quit()
```

### 5 Transplant to Target System

Outside of the container:
```bash
cd centos6.8/python_virtualenvs
tar cvzf test_pythonenv.tar.gz test/

cd pyenv/versions
tar cvzf 3.6.15.tar.gz 3.6.15
```

Copy both tarballs to the target system and extract them into the same paths as they existed inside Docker.



# Additional notes:
* It is not necessary to copy the complete pyenv project folder, just the python binaries it creates (EG: versions/3.6.15), along with the virtual envornments that virtualenv creates (EG test)
* Centos 7.x only support Python up to 3.9. To go higher, in the 3.10 range requires installing a newer version of OpenSSL, which would also have to be installed on target systems.
* Centos 6.8 Only supports up to 3.6.15
* The project uses git sub-modules for each environment. So for example to update Ubuntu24.04s:
'''
cd ubuntu24.0.4/python_virtualenvs/pyenv
git pull
'''
* You can see what versions of Python are available for attempted install in each container using:
'''
cd /python_virtualenvs/pyenv/bin/
./pyenv install --list
'''
It is best to run that from within the docker environment.

Enjoy!

🎉 PyFoundry simplifies Python portability — build once, run anywhere.

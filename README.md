# H1 -  PyFoundry
Pyfoundry docker environment for building standalone Python environments for system transplantation. It is useful in the folowing scenarios:
* Transplantation to air gapped systems with no internet connectivity.
* Systems where you want a clean pip driven Python without system Python packages (Ubuntu?),
* Instances where you don't want to disturb a Python environment on a production project, or have tainted libraries that you don't want to disturb. Simple package installs can trigger updates that will brake things.
* Would like a newer or older working version of Python than your operating system distribution might support out of the box.

The project uses: https://github.com/pyenv/pyenv

in conjunction with: https://virtualenv.pypa.io/en/latest/

To actually manage the virtual environments. The docker system functions as a build platform.

In order for the transplanted environment to function properly it is necessary to build python against the system for which it will run.

# H2 - Getting started and working with the system
* For starters you'll need to build a docker container for whatever platform you're compiling Python against. The following example assumings Ubuntu18.04:
'''
cd ubuntu18.04
./compile
'''
That will build the container, and enter it. From there you can start generating Python environments.

* This assumes you want to build 3.6.15 in a project called test.
'''
newenv 3.6.15 test
'''
Which will download, compile and install a Python 3.6.15 base environment to: /python_virtualenvs/versions, and a project folder:/python_virtualenvs/test, which is where Python and pip should be ran from and is the target directory for which Python modules will be installed.

* To install the paramiko module via pip simply run: 
'''
cd /python_virtualenvs/test/bin
./pip install --only-binary=:all: paramiko
'''
Using the --only-binary=:all: forces pip to use binary packages, which prevents the use of Tylenol.

* To test, from that same directory:
'''
./python
import paramiko
quit()
'''
* To transplant Python to the target system it is easier to start outside of the container you will need to use tar, like so:
'''
cd centos6.8/python_virtualenvs
tar cvzf test_pythonenv.tar.gz test/
cd pyenv/versions
tar cvzf 3.6.15.tar.gz 3.6.15
'''
copy both tarballs to the target system, and extract them to the same exact paths they exist within the docker container.




# H2 - Additional notes:
* It is not necessary to copy the complete pyenv project folder, just the python binaries it creates (EG: versions/3.6.15), along with the virtual envornments that virtualenv creates (EG test)
* Centos 7.x only support Python up to 3.9. To go higher, in the 3.10 range requires installing a newer version of OpenSSL, which would also have to be installed on target systems.
* Centos 6.8 Only supports up to 3.6.15
* The project uses git sub-modules for each environment. To update them simply go into python_virtualenv/pyenv, and run a 
'''
git fetch
git pull
'''
* You can see what versions of Python are available for attempted install in each container using:
'''
cd /python_virtualenvs/pyenv/bin/
./pyenv install --list
'''

ENJOY!!

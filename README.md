Pyfoundry docker environment for building standalone Python environments for system transplantation. It is useful in the folowing scenarios:
1)Transplantation to air gapped systems with no internet connectivity.
2)Systems where you want a clean pip driven Python without system Python packages (Ubuntu?),
3)Instances where you don't want to disturb a Python environment on a production project, or have tainted libraries that you don't want to disturb. Simple package installs can trigger updates that will brake things.
4)Would like a newer or older working version of Python than your operating system distribution might support out of the box.

The project uses:
https://github.com/pyenv/pyenv
in conjunction with:
https://virtualenv.pypa.io/en/latest/
To actually manage the virtual environments. The docker system functions as a build platform.

In order for the transplanted environment to function properly it is necessary to build python against the system for which it will run.

1)Within each directory is a 'compile' bash script, simply run this to build, start and enter the container. Note this will create and install a network in your docker environment for the containers use. Afterward you can easily enter your docker container with the enter bash script in each folder. This is just for convenience.
2)Once inside the container a bash script (in the path, so can be ran from anywhere) called 'newenv' can be used like so:
'''
newenv 3.6.15 test
'''
Which will download, compile and install a Python 3.6.15 base environment to: /python_virtualenvs/versions, and a project folder:/python_virtualenvs/test, which is where Python and pip should be ran from and is the target directory for which Python modules will be installed.

3)To install the paramiko module via pip simply run: 
'''
cd /python_virtualenvs/test/bin
./pip install --only-binary=:all: paramiko
'''
Using the --only-binary=:all: forces pip to use binary packages, which prevents the use of Tylenol.

4)To test, from that same directory:
'''
./python
import paramiko
quit()
'''
5)Transplanting: To transplant Python to the target system it is easier to start outside of the container you will need to use tar, like so:
'''
cd centos6.8/python_virtualenvs
tar cvzf test_pythonenv.tar.gz test/
cd pyenv/versions
tar cvzf 3.6.15.tar.gz 3.6.15
'''
copy both tarballs to the target system, and extract them to the same exact paths they exist within the docker container.

ENJOY! :)



It is not necessary to copy the complete pyenv project folder, just the python binaries it creates (EG: versions/3.6.15), along with the virtual envornments that virtualenv creates (EG test)

**Update:
**Centos 7.x only support Python up to 3.9. To go higher, in the 3.10 range requires installing a newer version of OpenSSL, which would also have to be installed on target systems.

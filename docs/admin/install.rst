==========================
Downloading and Installing
==========================

Downloading
===========
For moin2, there is currently no packaged download available, so you have to get
it from the repository.
You can use one of two repository URLs and either use Mercurial to keep a 
constantly updated copy of the code or download an archive of the files in tar.gz format:

Using Mercurial to clone one of the repositories::

 hg clone http://hg.moinmo.in/moin/2.0 moin-2.0
 OR
 hg clone http://bitbucket.org/thomaswaldmann/moin-2.0 moin-2.0

Now make sure your work directory is using the default branch::

 hg up -C default

Alternatively, visit http://hg.moinmo.in/moin/2.0 with your web browser and download the archive
(usually for the "default" branch) and unpack it.

Installing
==========
Before you can run moin, you need to install it:

Developer install
-----------------
Using your standard Python install or a virtualenv directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Please make sure you have `virtualenv` installed, it includes `pip`.

If you just want to run moin in-place in your mercurial working directory
with your normal system installation of Python, run the following command
from your mercurial moin2 working directory. You should not run this as an
administrator or root user; use your standard user account::

 ./quickinstall  # for Linux or other POSIX OSes
 # or
 quickinstall.bat  # for Windows

This will use virtualenv to create a directory `env/` within the current directory,
create a virtual environment for MoinMoin and then install moin2 including all dependencies into that directory.
`pip` will automatically fetch all dependencies from PyPI and install them, so this may take a while.
It will also compile the translations (`*.po` files) to binary `*.mo` files.

Please review the output of the quickinstall script, and check whether there were fatal errors.

If you have a bad network connection that makes the downloads fail, you can try to do the steps in quickinstall manually.

Further, the quickinstall script will create a `moin` script for your
platform which you can use for starting the built-in server or invoke moin script commands.

After you activated the virtual environment, the built-in server script, which is named 
`moin`, will be in the standard PATH, so you can just run the command `moin` on the command line.

**Note:** in this special mode, it won't copy the MoinMoin code to the env/ directory,
it will run everything from your work dir, so you can modify code and directly try it out.
You only need to do this installation procedure once.

Using a different Python or a different virtualenv directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For example, if you want to use `PyPy` and want to name the virtualenv directory `env-pypy`,
use::

 # for linux
 DIR=env-pypy
 PYTHON=/opt/pypy/bin/pypy

That way, you can test with different versions of Python in different virtualenv directories within your moin2 workdir.

Activating the virtual env
--------------------------

IMPORTANT: you always need to activate the virtual environment before running
anything that executes moin code! Otherwise it won't find the moin command,
nor the moin code nor the libraries it needs. Also, if you want to install
additional software into the virtual environment, activate it before running pip!::

 source env/bin/activate  # for linux (or other posix OSes)
 # or
 env\Scripts\activate.bat  # for windows

As you have activated the virtual env now, the moin command should be in your
path now, so you can invoke it using "moin".

Letting moin find the wiki configuration
----------------------------------------

moin needs to find the wiki configuration. If you want it to run in the most
simple way without giving parameters to the moin command, it is easiest if
you are in the same directory as the configuration files, e.g. wikiconfig.py.

If you are working from a repository workdir, this is the top level
directory and there is already a ready-to-use wikiconfig.py.

In case you want to just give the configuration file location, make sure you
use an **absolute path**. moin will try to find its configuration in this
order:

- command line argument `--config /path/to/wikiconfig.py`
- environment variable `MOINCFG=/path/to/wikiconfig.py`
- current directory, file `wikiconfig_local.py`
- current directory, file `wikiconfig.py`

Initializing index and/or storage
---------------------------------
If you have an existing storage AND a valid index (for this storage???s content and for this moin version),
you can skip this section.

If you start from scratch, ie no storage and no index created yet,
you need to create an empty storage and an empty index::

 # create storage and index:
 moin index-create -s -i

Loading some items
------------------
If you don't want to have a completely empty wiki, you can optionally load
some example items into the storage like this::

 # load some example items:
 moin load --file contrib/serialized/items.moin

Building the index
------------------
If you have some items in your storage, but no index built yet, you need
to build an index::

 moin index-build


Installing PIL
~~~~~~~~~~~~~~
For some image processing functions that MoinMoin uses like resizing and rotating,
you need PIL, which is the Python Imaging Library.

Windows users who want to install PIL should skip the remainder of this section and read
Troubleshooting -- PIL Installation Under Windows below.

If you install PIL with pip, then pip will try to find a jpeg support library and associated development
headers on your system and if you do not have them, there will be no jpeg support in PIL.
So, if you want jpeg support, make sure you have the jpeg libs/headers::

 # install jpeg library and development headers:
 sudo apt-get install libjpeg62-dev  # Ubuntu / Debian-based
 yum install libjpeg-turbo-devel  # Fedora / Redhat-based

Now activate your virtual environment and install PIL into it::

 pip install pil # for Linux or other POSIX OSes

Troubleshooting
---------------

PyPi down
~~~~~~~~~
Now and then, PyPi might be down or unreachable.

There are mirrors b.pypi.python.org, c.pypi.python.org, d.pypi.python.org
you can use in such cases. You just need to tell pip to do so::

 # put this into ~/.pip/pip.conf
 [global]
 index-url = http://c.pypi.python.org/simple

In case that doesn't work either, try our mini pypi that should have all
packages you need for moin::

 # put this into ~/.pip/pip.conf
 [global]
 index-url = http://pypi.moinmo.in/simple

Bad Network Connection
~~~~~~~~~~~~~~~~~~~~~~
If you have a poor or limited network connection, you may run into trouble with the commands issued by
the quickinstall script.
You may see tracebacks from pip, timeout errors, etc. See the output of the quickinstall script.

If this is the case, try it manually::

 # enter your virtual environment:
 source env/bin/activate

 # confirm the problems by running:
 pip install -e .

Now install each package into your virtual env manually:

* Find the required packages by looking at "install_requires" within `setup.py`.
* Download each required package from http://pypi.python.org/
* Install each of them individually::

    pip install package.tar

* Now try again::

    pip install -e .

Repeat these steps until you don't see fatal errors.

PIL Installation Under Windows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
PIL version 1.1.7 does not install correctly via "pip install pil" on Windows.
Some users have had success using "pip install pillow", a fork of PIL fixing
a packaging issue. Other users have resorted to installing PIL 1.1.6 in the
main Python directory using the Windows installers available at
http://www.pythonware.com/products/pil/


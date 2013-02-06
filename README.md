About
=====
This repository contains code relating to piface projects.

[Getting Started](https://docs.google.com/document/d/145TkSMwnPAJaqKMLxdvD8aGULQ8UgIIU3hg-JAKwAa0/edit)

[Documentation](https://docs.google.com/folder/d/0B-UAZ9CyJCLGQjJ3RDlqa2pqaDg/edit)

[Downloads](http://pi.cs.man.ac.uk/download)

**PiFace uses Python 2 and is incompatable with Python 3.**

Installation and Setup
======================
**Ignore the following steps if you have installed the custom
Raspberry Pi SDCard image (see [Getting Started](https://docs.google.com/document/d/145TkSMwnPAJaqKMLxdvD8aGULQ8UgIIU3hg-JAKwAa0/edit))**

These instructions assume you are on working on a Raspberry Pi with a 
kernel that supports the SPI devices (we're using [bootc](http://www.bootc.net/)).
and that the user 'pi' has read/write access to /dev/spidev*. This can
be set up using the spidev_setup.sh script in scripts/.

### Dependencies
Everything (if you're unsure, just run this one)

    # apt-get install python-dev python-gtk2-dev git automake libtool espeak python-django python-simplejson

Just Python

    # apt-get install python-dev python-gtk2-dev git

Just C

    # apt-get install automake libtool

Easyteach:

    # apt-get install espeak

Web Interface

    # apt-get install python-django python-simplejson
    
### Getting the source
To download all of the source files simply run the following command:

    $ git clone https://github.com/thomasmacpherson/piface.git

This will create a directory called *piface*, which contains the entire
project tree stored in this Git repository.

### Python
To install the piface python package you must first download the source,
move into the piface directory and then run the setup script as root (ignore
the compiler warnings about data types not matching up, it's just being
picky!):

    $ cd piface/python/
    $ sudo python setup.py install

Now whenever you enter a python shell you can access the piface's
input/output module or the emulator by importing them like so:

    $ python
    >>> import piface.pfio
    >>> piface.pfio.init()
    >>> piface.pfio.digital_write(1, 1)
    >>> led1 = piface.pfio.LED(1)
    >>> led1.turn_off()
    >>> import piface.emulator
    >>> piface.emulator.init()

*Note: Make sure you have left the python source directory before importing
any modules, otherwise Python may get confused*

If you prefer, you can refer to the pfio and emulator modules directly
using the following Python syntax:

    $ python
    >>> import piface.pfio as pfio
    >>> import piface.emulator as emulator
    >>> pfio.digital_write(4, 0)
    >>> emulator.digital_write(3, 1)

The emulator can be used in the same way as the pfio. Upon initialisation,
an image of the Pi Face will be drawn to a window and its inputs/outputs
can be seen.

You can find some example python scripts in python/demos.

### C
To install the C pfio library download the source, move into the C directory,
call the setup scripts and then (as root) run the install command:

    $ cd piface/c/
    $ ./autogen.sh && ./configure && make && sudo make install
    
To use the C libraries you will need to include the pfio header file from 
the piface library and then compile with the correct flags:

    $ cat piface_program.c
    #include <libpiface-1.0/pfio.h>

    int main(void)
    {
       pfio_init();
       pfio_digital_write(1, 1);
       pfio_deinit();
    }
    $ gcc -L/usr/local/lib/ -lpiface-1.0 -o piface_program piface_program.c
    $ ./piface_program

Testing
=======
### Python
To test the installed piface package run the tests in the tests/ directory.

    $ cd python/tests/
    $ python piface_test.py

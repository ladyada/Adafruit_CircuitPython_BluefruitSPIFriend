Introduction
============

.. image:: https://readthedocs.org/projects/adafruit-circuitpython-bluefruitspi/badge/?version=latest
    :target: https://circuitpython.readthedocs.io/projects/bluefruitspi/en/latest/
    :alt: Documentation Status

.. image:: https://img.shields.io/discord/327254708534116352.svg
    :target: https://discord.gg/nBQh6qu
    :alt: Discord

.. image:: https://travis-ci.org/adafruit/Adafruit_CircuitPython_BluefruitSPI.svg?branch=master
    :target: https://travis-ci.org/adafruit/Adafruit_CircuitPython_BluefruitSPI
    :alt: Build Status

Helper class to work with the Adafruit Bluefruit LE SPI Friend.

Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_
* `Bus Device <https://github.com/adafruit/Adafruit_CircuitPython_BusDevice>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://github.com/adafruit/Adafruit_CircuitPython_Bundle>`_.

Usage Example
=============

.. code-block:: python

    import busio
    import digitalio
    import board

    from adafruit_bluefruitspi import BluefruitSPI, MsgType

    spi_bus = busio.SPI(board.SCK, MOSI=board.MOSI, MISO=board.MISO)
    cs = digitalio.DigitalInOut(board.A5)
    irq = digitalio.DigitalInOut(board.A4)
    bluefruit = BluefruitSPI(spi_bus, cs, irq, debug=True)

    # Send the ATI command
    try:
        msgtype, msgid, rsp = bluefruit.cmd("ATI\n")
        if msgtype == MsgType.ERROR:
            print("Error (id:{0})".format(hex(msgid)))
        if msgtype == MsgType.RESPONSE:
            print("Response:")
            print(rsp)
    except RuntimeError as error:
        print("AT command failure: " + repr(error))
        exit()

Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_BluefruitSPI/blob/master/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.

Building locally
================

Zip release files
-----------------

To build this library locally you'll need to install the
`circuitpython-build-tools <https://github.com/adafruit/circuitpython-build-tools>`_ package.

.. code-block:: shell

    python3 -m venv .env
    source .env/bin/activate
    pip install circuitpython-build-tools

Once installed, make sure you are in the virtual environment:

.. code-block:: shell

    source .env/bin/activate

Then run the build:

.. code-block:: shell

    circuitpython-build-bundles --filename_prefix adafruit-circuitpython-bluefruitspi --library_location .

Sphinx documentation
-----------------------

Sphinx is used to build the documentation based on rST files and comments in the code. First,
install dependencies (feel free to reuse the virtual environment from above):

.. code-block:: shell

    python3 -m venv .env
    source .env/bin/activate
    pip install Sphinx sphinx-rtd-theme

Now, once you have the virtual environment activated:

.. code-block:: shell

    cd docs
    sphinx-build -E -W -b html . _build/html

This will output the documentation to ``docs/_build/html``. Open the index.html in your browser to
view them. It will also (due to -W) error out on any warning like Travis will. This is a good way to
locally verify it will pass.

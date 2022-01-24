Introduction
============

.. image:: https://readthedocs.org/projects/adafruit-circuitpython-il0373/badge/?version=latest
    :target: https://docs.circuitpython.org/projects/il0373/en/latest/
    :alt: Documentation Status

.. image:: https://img.shields.io/discord/327254708534116352.svg
    :target: https://adafru.it/discord
    :alt: Discord

.. image:: https://github.com/adafruit/Adafruit_CircuitPython_IL0373/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_IL0373/actions
    :alt: Build Status

CircuitPython `displayio` driver for IL0373-based ePaper displays


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://github.com/adafruit/Adafruit_CircuitPython_Bundle>`_.

Installing from PyPI
=====================
On supported GNU/Linux systems like the Raspberry Pi, you can install the driver locally `from
PyPI <https://pypi.org/project/adafruit-circuitpython-il0373/>`_. To install for current user:

.. code-block:: shell

    pip3 install adafruit-circuitpython-il0373

To install system-wide (this may be required in some cases):

.. code-block:: shell

    sudo pip3 install adafruit-circuitpython-il0373

To install in a virtual environment in your current project:

.. code-block:: shell

    mkdir project-name && cd project-name
    python3 -m venv .env
    source .env/bin/activate
    pip3 install adafruit-circuitpython-il0373

Usage Example
=============

.. code-block: python

    """Simple test script for 2.13" 212x104 tri-color featherwing.

    Supported products:
      * Adafruit 2.13" Tri-Color FeatherWing
        * https://www.adafruit.com/product/4128
      """

    import time
    import board
    import displayio
    import adafruit_il0373

    displayio.release_displays()

    epd_cs = board.D9
    epd_dc = board.D10

    display_bus = displayio.FourWire(board.SPI(), command=epd_dc, chip_select=epd_cs, baudrate=1000000)
    time.sleep(1)

    display = adafruit_il0373.IL0373(display_bus, width=212, height=104, rotation=90,
                                     highlight_color=0xff0000)

    g = displayio.Group()

    f = open("/display-ruler.bmp", "rb")

    pic = displayio.OnDiskBitmap(f)
    # CircuitPython 6 & 7 compatible
    t = displayio.TileGrid(
        pic, pixel_shader=getattr(pic, "pixel_shader", displayio.ColorConverter())
    )
    # CircuitPython 7 compatible only
    # t = displayio.TileGrid(pic, pixel_shader=pic.pixel_shader)
    g.append(t)

    display.show(g)

    display.refresh()

    print("refreshed")

    time.sleep(120)

Documentation
=============

API documentation for this library can be found on `Read the Docs <https://docs.circuitpython.org/projects/il0373/en/latest/>`_.

Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_IL0373/blob/main/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.

Documentation
=============

For information on building library documentation, please check out `this guide <https://learn.adafruit.com/creating-and-sharing-a-circuitpython-library/sharing-our-docs-on-readthedocs#sphinx-5-1>`_.

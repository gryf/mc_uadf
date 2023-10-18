=======================
Midnight Commander uadf
=======================

Midnight Commander extfs plugin for handling Amiga adf/dms floppy images.

Description
-----------

UAdf is an extfs plugin suitable for reading .adf, .adz and .dms Amiga floppy
disk images. Due to limitations of the
`unadf <http://freecode.com/projects/unadf>`_, file access inside disk image is
read only.

In case of corrupted or no-dos images, message will be shown.


Requirements
------------

This script is using ``unadf`` v1.2 utility from `ADFlib
<https://github.com/lclevy/ADFlib>`_ package in version 0.8. Version of unadf
can be check by simply issuing unadf without arguments:

.. code:: shell-session

   $ unadf

If it turns out that your distribution doesn't provide proper version of
ADFlib, there will be a need for building it by hand.

It may be done by using following steps:

#. Grab the `sources <https://github.com/lclevy/ADFlib>`_
#. Build and install it, using instructions from `INSTALL
   <https://github.com/lclevy/ADFlib/blob/master/INSTALL>`_ file.

For optional dms support, `xdms <http://zakalwe.fi/~shd/foss/xdms/>`_ utility 
is needed.

Installation
------------

* install `extfslib`_
* copy ``uadf`` to ``~/.local/share/mc/extfs.d/``
* add or change entry for files handle in ``~/.config/mc/mc.ext.ini``:

.. code:: ini

   [adf]
   Type=^Amiga\ .* disk
   Open=%cd %p/uadf://
   View=%view{ascii} unadf -lrm %f 2>/dev/null

   [adz]
   Regex=\.adz$
   View=%view{ascii} t=$(mktemp --suffix .adf); zcat %f > ${t}; unadf -lrm ${t} 2</dev/null; rm ${t}
   Open=%cd %p/uadf://

   [dms]
   Regex=\.dms$
   View=%view{ascii} t=$(mktemp --suffix .adf); xdms u %f "+${t}" 2>/dev/null; unadf -lrm ${t} 2</dev/null; rm ${t}
   Open=%cd %p/uadf://


License
=======

This software is licensed under 3-clause BSD license. See LICENSE file for
details.


.. _extfslib: https://github.com/gryf/mc_extfslib

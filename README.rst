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

It requires ``unadf`` utility from `ADFlib <https://github.com/lclevy/ADFlib>`_
repository.

If it turns out that your distribution doesn't provide proper version of ADFlib,
there will be a need for building it by hand.

It may be done by using following steps:

#. Grab the `sources
   <http://http.debian.net/debian/pool/main/u/unadf/unadf_0.7.11a.orig.tar.gz>`_
   and `patches
   <http://http.debian.net/debian/pool/main/u/unadf/unadf_0.7.11a-3.debian.tar.gz>`_
   from `Debian repository <http://packages.debian.org/sid/unadf>`_.
#. Extract ``unadf_0.7.11a-3.debian.tar.gz`` and ``unadf_0.7.11a.orig.tar.gz``
   into some temporary directory::

   $ mkdir temp
   $ cd temp
   $ tar zxf ~/Downloads/unadf_0.7.11a-3.debian.tar.gz
   $ tar zxf ~/Downloads/unadf_0.7.11a.orig.tar.gz
   $ cd unadf-0.7.11a

#. Apply Debian patches::

    $ for i in `cat ../debian/patches/series`; do
    > patch -Np1 < "../debian/patches/${i}"
    > done

#. Apply the patch from extras directory::

   $ patch -Np1 < [path_to_this_repo]/extras/unadf_separate_comment.patch
   $ make
   $ cp Demo/unadf [destination_path]

#. Place ``unadf`` binary under directory reachable by ``$PATH``.

For optional dms support, `xdms <http://zakalwe.fi/~shd/foss/xdms/>`_ utility is
needed.

Installation
------------

* install `extfslib`_
* copy ``uadf`` to ``~/.local/share/mc/extfs.d/``
* add or change entry for files handle in ``~/.config/mc/mc.ext``::

    # adf
    type/^Amiga\ .* disk
        Open=%cd %p/uadf://
        View=%view{ascii} unadf -lr %f

    # adz
    regex/\.([aA][dD][zZ])$
        Open=%cd %p/uadf://

    # dms
    regex/\.([dD][mM][sS])$
        Open=%cd %p/uadf://

License
=======

This software is licensed under 3-clause BSD license. See LICENSE file for
details.


.. _extfslib: https://github.com/gryf/mc_extfslib

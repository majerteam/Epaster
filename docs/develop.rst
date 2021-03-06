.. _intro_develop:
.. _buildout: http://www.buildout.org/
.. _virtualenv: http://www.virtualenv.org/
.. _Sphinx: http://sphinx-doc.org/
.. _Foundation 3: http://foundation.zurb.com/old-docs/f3/
.. _Foundation: http://foundation.zurb.com/

***********
Development
***********

For Epaster (or its pastes) development, you will need to install the Epaster repository, then use the development environnment  : ::

    git clone https://github.com/emencia/Epaster.git
    make install
    source bin/activate
    buildout -c development.cfg

It will requires that you have installed the GIT client because the development environnment installs the pastes sources in the ``src/`` directory. This is where you will work.

Documentation
=============

The documentation sources lives in the ``docs/`` as a `Sphinx`_ project, `Sphinx`_ is installed within the development environnment.

Note that for each paste the *mods* documentation is automatically builded from the paste. For example with the ``emencia_paste_djangocms_3`` paste, a ``docs/emencia_paste_djangocms_3.rst`` file is builded from its source code.

The paste's mods documentation is not builded from the common ``make html`` Sphinx command, but with the dedicated command : ``make grab``.

The *grab* action will take it's base document from ``project/mods_available/__init__.py`` docstring then replace the directive ``.. document-mods::`` with the grabbed mods documentation.

The grabbed mods documentation itself is taken from each mod living in ``project/mods_available/`` using their ``__init__.py`` docstring, then they are assembled as an unique string that will replace the ``.. document-mods::`` directive.

**Beware** that all these ``__init__.py`` **docstrings must be valid RST syntax** else it will break the documentation building.

Finally to ease the documentation building, when you did lot of changes in the mods documentation, just use the following command to rebuild their docs then build the whole project documentation : ::

    make all

This command assemble the ``make grab`` and ``make html`` commands.

Symlinks
========

You can't include symlinks into your paste templates, because Distribute ignore them, they won't be packaged and so won't be available in the paste's installed egg.

If you need to create some symlinks in the projects to build, you will have to do it in the paste template in ``templates.py``. The ``emencia_paste_djangocms_3`` paste has generic way to do this, just append a tuple to the list ``emencia_paste_djangocms_3.templates.Django.symlink_list`` where the tuple contains the *target* (a relative path to the file/directory to link to) and the symlink file to create (an absolute path into the project to build).

Foundation updates
==================

.. warning::
        To do this, you must have a strong knowing of Foundation sources structure
        
        You only have to do this when you want to synchronize the project's Foundation sources from the latest Foundation release. This is reserved for Epaster maintainers.

This project embeds `Foundation`_ 5 sources installed from the `Foundation`_ app so you can update it from the sources if needed (and if you have installed the Foundation cli, see its documentation for more details). to update these sources go into its directory and use it Makefile action: ::

    make update

.. warning::
    Never manually put your files in the ``project/webapp_statics/js/foundation5`` directory or they will be deleted.

Then you have to checkup that directories structure has not changed, if it so you must fix the ``syncf5`` and ``syncjquery`` actions. When it's done, do: ::

    make syncf5

For the SCSS sources, no action is required; they are imported directly into the compass config.

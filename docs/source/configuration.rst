DXR Configuration
=================

*This document describes how to configure your DXR builds, as well as
fully documenting all configuration settings.*

Configuration File Format
-------------------------

The configuration file is split into different sections, sections
``DXR`` is special; all other sections describes a tree for indexing.
Keys that must and can be specified for different sections are listed in
relevant section below.

The config file must be written in a format compatible with python's
built-in
`ConfigParser <http://docs.python.org/library/configparser.html>`__, see
python documentation for details.

DXR Configuration
-----------------

*Configuration keys for the* ``DXR`` *section of the configuration file.*

-  ``target_folder`` Output directory (**required**)
-  ``dxrroot`` Location of you DXR install (defaults to directory of the
   executable)
-  ``plugin_folder`` Plugin folder (default ``<dxrroot>/plugins``)
-  ``nb_jobs`` Number of jobs allowed (default ``1``), note that this
   value can be overwritten by the ``-j`` argument when running
   ``dxr-build.py``
-  ``temp_folder`` Temporary folder (default ``/tmp/dxr-temp``)
-  ``log_folder`` Log folder (default ``<temp_folder>/logs``)
-  ``wwwroot`` Virtual root on deployment server (default ``/``)
-  ``enabled_plugins`` Enabled plugins (default ``*``)
-  ``disabled_plugins`` Disabled plugins (default ``''``)
-  ``directory_index`` Filename for directory index files (default
   ``.dxr-directory-index.html``)
-  ``generated_date`` In RFC-822 format, also know as (RFC 2822), You
   might want to overwrite this to get the same date on all files!
   (defaults to time of execution)
-  ``skip\_stages`` Build/indexing stages to skip (default ``''``). Can be
   zero or more of 'index', 'html' (space separated)
-  ``disable\_workers`` If non-empty, do not use a worker pool for
   htmlification (default ``''``)
-  ``filter\_lang`` The default (programming) language for this instance.
   Only filters registered for this language will be used (default ``'C'``)

(Refer to the Plugin Configuration section for plugin keys available
here).

Notice, it's strongly recommended that you provide a sane **temporary
folder**, using ``/tmp`` (default) is not recommended. Creating a
sub-directory under ``/tmp`` is better, but preferable you'd want
something on the same file system as the ``target_folder``.

Please note that ``dxr-build.py`` assume the plugins in
``plugin_folder`` are already built and ready for use. If you specify
your own plugin folder, the top-level makefile will not do this for you.
This setting is mainly here for completion, it's not recommended for
normal operation.

You might be tempted use ``index.html`` for ``directory_index``,
however, this is **not** recommened as any indexed files with that name
would hide the directory index, which must be rather confusing to the
users. (Note: this value will also be used in the generated
``.htaccess`` file.)

Tree Configuration
------------------

*Configuration keys for tree sections of the configuration file.*

Any section that is not named ``DXR`` specifies a tree using the keys
listed below.

-  ``source_folder`` Source folder (**required**)
-  ``build_command`` Command for building sources (defaults to
   ``make -j $jobs``) Notice that ``$jobs`` will be replaced with
   ``nb_jobs`` as provided in the config file or defined at runtime
   using the ``-j`` argument.
-  ``object_folder`` Folder where object files will be stored
   (**required**)
-  ``log_folder`` Log folder (defaults ``<config_log_folder>/<tree>``)
-  ``temp_folder`` Tempoary folder for this tree, (defaults
   ``<config_temp_folder>/<tree>``) It's **not** recommend that this
   attribute is defined!
-  ``enabled_plugins`` Plugins enabled in this tree (defaults ``*``)
-  ``disabled_plugins`` Plugins disabled in this tree (default ``*``),
   note that plugins disabled in the ``DXR`` section is also disabled.
-  ``ignore_patterns`` Space separated list of Unix shell-style file
   patterns to ignore, for information on the pattern style, see
   `fnmatch <http://docs.python.org/library/fnmatch.html>`__

(Refer to the Plugin Configuration section for plugin keys available
here).

Notice that is strongly recommended that you don't define
``temp_folder``; this feature is just here for completeness. If you
define a ``build_command`` without using ``$jobs`` somewhere in the
command, you will be warned, but the build will continue.

Plugin Configuration
--------------------

Keys prefixed with ``plugin_`` in either a tree section or the DXR
section will read and stored on the ``config`` and ``tree`` objects,
available to plugins. These keys can be used to configure plugins, and
some plugins might require these in order to work. Plugin developers are
advised to name their configuration keys as
``plugin_<plugin-name>_<key>``, refer to Plugins.mkd for more details on
plugin development.

Plugin keys listed below can be defined in the tree sections of the
configuration file.

-  ``plugin_hg_hgweb`` Location of hg-web installation for this tree.
   (Assumed to have default url layout)
-  ``plugin_hg_section`` Section text (defaults to ``Mercurial``)
-  ``plugin_buglink_name`` Name of the projects bug tracker
   installation. (eg. ``bugzilla.mozilla.org``)
-  ``plugin_buglink_bugzilla`` URL pattern for buglinks, %s will be
   replaced with the bug number, this key must include ``http://``


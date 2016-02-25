.. include:: globals.rst

-------------
Customization
-------------

The plugin can be customized through the use of system wide or global |qg| settings.  As explained in the `PyQGIS Developer Cookbook <http://documentation.qgis.org/2.8/de/docs/pyqgis_developer_cookbook/settings.html>`_ :

.. pull-quote::
   ... global settings [are stored] in system’s “native” way of storing settings, that is — registry (on Windows), .plist file (on Mac OS X) or .ini file (on Unix).

Currently, it is possible to customize the behaviour of the plugin in two ways :

#. Change the url pointed to by the *help* button (|help_png|) through the 'qgis-versioning/help-url' setting
#. Disable the use of branches other than *trunk* through the 'qgis-versioning/use-branches' setting

Defaults for those two global settings are :

1) allow using branches other than trunk
2) point the help button to the official plugin documentation site

.. note::
   If a particular setting is not defined on the user's system (e.g. in windows the registry keys do not exist), or if values are not valid (e.g. a non valid url), then defaults are used.  Values for the 'qgis-versioning/use-branches' setting needed to disable branches must be either "False" or "false".  Any other value, including *None* or an empty string, defaults to "true" (enable branches).  Changing those settings will have an effect only upon reloading the plugin, which requires a |qg| restart.

When branching is disabled, no branch button shows up in the menu.  The menu bar for a versioned database layer group with branches allowed vs disallowed is shown below :

|versioned_menu_branch_yes_png|

|versioned_menu_branch_no_png|

It is important to understand that the plugin does not require customization; it merely provides facilities for doing so.  The next sections explain how to customize the plugin.

How to
======
A |qg| user that has administrative privileges on his(her) computer can control the settings in a number of ways.  For brevity, we will limit our discussion to MS Windows users.

|qg| stores system wide settings in the Windows registry under the *HKEY_CURRENT_USER\\SOFTWARE\\QGIS\\QGIS2* registry key.

.. warning::
   |qg| settings in the windows registry apply to all point versions of the 2.x series installed by a particular user.

In a typical |qg| installation, the registry will show those subkeys under *QGIS2* :

|clean_registry_png|

A user can either create subkeys for the plugin directly in the registry or use the |qg| Python console |python_console_icon_png|.  In the console, type the following commands :

.. code:: python

   from PyQt4.QtCore import QSettings
   QSettings().setValue('qgis-versioning/use-branches', "False") (or False or "false") # If you want to disable branches
   QSettings().setValue('qgis-versioning/help-url', "https://your_docs_website") # If you want to point to a site other than the plugin's official ReadTheDocs site

The result in |qg| is depicted below :

|python_console_qsettings_png|

Once you have done that, open the Windows registry again and see that two new subkeys were added under *qgis-versioning*:

|registry_with_settings_png|

In that example, we see that branches are enabled and that the url chosen for the documentation is that of python.org.  After |qg| is restarted, hovering over the *help* button will show that new url :

|help_url_tiptool_png|

At this point, the user may change the values of the settings either through the Python console or in the registry.  Users that do not have access to the registry will need to contact their systems administrator.

.. note::
   Customization settings are probably more useful to systems administrators than to individual QGIS users.  Organizations may for example restrict their |qg| users not to use branches other than trunk.  Or they may want to provide their own internal web page for users seeking help using the plugin. Systems administrators managing |qg| wokstations with Active Directory will easily deploy company wide settings for the |plugin| through group policies.

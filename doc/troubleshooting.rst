Appendix C: Troubleshooting
===========================

The following two general issues can result in failed synchronization:

- The server setup is incorrect.
- The client contains a bug. 

When reporting bugs, it is helpful if you first determine what part of the
system is causing the issue.

Identifying Basic Functionality Problems
----------------------------------------

:Performing a general ownCloud Server test:
  The first step in troubleshooting synchronization issues is to verify that
  you can log on to the ownCloud web application. To verify connectivity to the
  ownCloud server:

  - Open a browser window and enter the server address to your own server in the location/address bar.

  For example, if your ownCloud instance is installed at URL address
  ``http://yourserver.com/owncloud``, enter ``http://yourserver.com/owncloud/``
  into your browsers location/address bar.
   
  If you are not prompted for your username and password, or if a red warning
  box appears on the page, your server setup requires modification. Please verify
  that your server installation is working correctly.

:Ensure the WebDAV API is working:
  If all desktop clients fail to connect to the ownCloud Server, but access
  using the web interface functions properly, the problem is often a
  misconfiguration of the WebDAV API.

  The ownCloud Client uses the built-in WebDAV access of the server content.
  Verify that you can log on to ownClouds WebDAV server. To verify connectivity
  with the ownCloud WebDAV server:

  - Open a browser window and enter the address to the ownCloud WebDAV server. 

  For example, if your ownCloud instance is installed at
  ``http://yourserver.com/owncloud``, your WebDAV server address is
  ``http://yourserver.com/owncloud/remote.php/webdav``.

  If you are prompted for your username and password but, after providing the
  correct credentials, authentication fails, please ensure that your
  authentication backend is configured properly.

:Use a WebDAV command line tool to test:  
  A more sophisticated test method for troubleshooting syncrhonization issues
  is to use a WebDAV command line client and log into the ownCloud WebDAV server.
  One such command line client -- called cadaver -- is available for Linux
  distributions. You can use this application to further verify that the WebDAV
  server is running properly using PROPFIND calls.  

  As an example, after installing the cadaver app, you can issue the
  ``propget`` command to obtain various properties pertaining to the current
  directory and also verify WebDAV server connection.


Isolating other issues
----------------------

Other issues can affect synchronization of your ownCloud files:

- If you find that the results of the synchronizations are unreliable, please
  ensure that the folder to which you are synchronizing is not shared with
  other synchronization applications.

  .. note:: Synchronizing the same directory with ownCloud and other
  synchronization software such as Unison, rsync, Microsoft Windows Offline
  Folders, or other cloud services such as DropBox or Microsoft SkyDrive is not
  supported and should not be attempted. In the worst case, it is possible that
  synchronizing folders or files using ownCloud and other synchronization
  software or services can result in data loss.

- If you find that only specific files are not synrchronized, the
  synchronization protocol might be having an effect. Some files are
  automatically ignored because they are system files, other files might be
  ignored because their filename contains characters that are not supported on
  certain file systems. For more information about ignored files, see
  :ref:`_ignored-files-label`.

- If you are operating your own server, and use the local storage backend (the
  default), make sure that ownCloud has exclusive access to the directory.

  .. note:: The data directory on the server is exclusive to ownCloud and must not be modified manually.

  If you are using a different file backend on the server, you can try to exclude a bug in the
  backend by reverting to the built-in backend.

Log Files
---------

Effectively debugging software requires as much relative information as can be
obtained.  To assist the ownCloud support personnel, please try to provide as
many relevant logs as possible. Log output can help  with tracking down
problems and, if you report a bug, log output can help to resolve an issue more
quickly.

Obtaining the Client Log File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To obtain the client log file:

1. Open the ownCloud Desktop Client.

2. Press F12 on your keyboard.

  The Log Output window opens.

  .. image:: images/log_output_window.png

3. Click the 'Save' button.

  The Save Log File window opens.

  .. image:: images/save_log_file.png

4. Migrate to a location on your system where you want to save your log file.

5. Name the log file and click the 'Save' button.

  The log files is saved in the location specifed.

Alternatively, you can launch the ownCloud Log Output window using the
``--logwindow`` command. After issuing this command, the Log Output window
opens to show the current log. You can then follow the same procedures
mentioned above to save the log to a file.

  .. note:: You can also open a log window for an already running session, by
     restarting the client using the following command:

  * Windows: ``C:\Program Files (x86)\ownCloud\owncloud.exe --logwindow``
  * Mac OS X: ``/Applications/owncloud.app/Contents/MacOS/owncloud --logwindow``
  * Linux: ``owncloud --logwindow``

Saving Files Directly
~~~~~~~~~~~~~~~~~~~~~

The ownCloud client enables you to save log files directly to a predefined file
or directory.  This is a useful option for troubleshooting sporadic issues as
it enables you to log large amounts of data and bypasses the limited buffer
settings associated with the log window.

To save log files to a file or a directory:

1. To save to a file, start the client using the ``--logfile <file>`` command,
   where ``<file>`` is the filename to which you want to save the file.

2. To save to a directory, start the client using the ``--logdir <dir>`` command, where ``<dir>``
   is an existing directory.

When using the ``--logdir`` command, each sync run creates a new file. To limit
the amount of data that accumulates over time, you can specify the
``--logexpire <hours>`` command. When combined with the ``--logdir`` command,
the client automatically erases saved log data in the directory that is older
than the specified number of hours.

As an example, to define a test where you keep log data for two days, you can
issue the following command:

```
owncloud --logdir /tmp/owncloud_logs --logexpire 48
```

ownCloud server Log File
~~~~~~~~~~~~~~~~~~~~~~~~

The ownCloud server also maintains an ownCloud specific log file. This log file
must be enabled through the ownCloud Administration page. On that page, you can
adjust the log level. We recommend that when setting the log file level that
you set it to a verbose level like ``Debug`` or ``Info``.
  
You can view the server log file using the web interface or you can open it
directly from the file system in the ownCloud server data directory.

.. todo:: Need more information on this.  How is the log file accessed?
   Need to explore procedural steps in access and in saving this file ... similar
   to how the log file is managed for the client.  Perhaps it is detailed in the
   Admin Guide and a link should be provided from here.  I will look into that
   when I begin heavily editing the Admin Guide.

Webserver Log Files
~~~~~~~~~~~~~~~~~~~

It can be helpful to view your webservers error log file to isolate any
ownCloud-related problems. For Apache on Linux, the error logs are typically
located in the ``/var/log/apache2`` directory. Some helpful files include the
following:

- ``error_log`` -- Maintains errors associated with PHP code. 
- ``access_log`` -- Typically records all requests handled by the server; very
  useful as a debugging tool because the log line contains information specific
  to each request and its result.
  
You can find more information about Apache logging at
``http://httpd.apache.org/docs/current/logs.html``.

Core Dumps
----------

On MAC OS X and Linux systems, and in the unlikely event the client software
crashes, the client is able to write a core dump file.  Obtaining a core dump
file can assist ownCloud Customer Support tremendously in the debugging
process. 

To enable the writing of core dump files, you must define the
``OWNCLOUD_CORE_DUMP`` environment variable on the system.

For example:

```
OWNCLOUD_CORE_DUMP=1 owncloud
```

This command starts the client with core dumping enabled and saves the files in
the current working directory.  

.. note:: Core dump files can be fairly large.  Before enabling core dumps on
   your system, ensure that you have enough disk space to accommodate these files.
   Also, due to their size, we strongly recommend that you properly compress any
   core dump files prior to sending them to ownCloud Customer Support.

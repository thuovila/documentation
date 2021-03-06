.. _plugin-obox:

===========
obox plugin
===========

``obox-plugin``
^^^^^^^^^^^^^^^

.. _plugin-obox-setting_obox_use_object_ids:

``obox_use_object_ids``
-----------------------

.. deprecated:: v2.3.0

- Default: ``no``
- Values: :ref:`boolean`

Access objects directly via their IDs instead of by paths, if possible. This
can bypass index lookups with Scality CDMI and fs-dictmap/Cassandra. This
setting was removed from v2.3 and made the default. (Although there is
:ref:`plugin-obox-setting_obox_dont_use_object_ids` to disable it if really
needed.)


.. _plugin-obox-setting_obox_track_copy_flags:

``obox_track_copy_flags``
-------------------------

- Default: ``no``
- Values: :ref:`boolean`

Enable only if dictmap/Cassandra & lazy_expunge plugin are used: Try to avoid
Cassandra SELECTs when expunging mails.


.. _plugin-obox-setting_obox_refresh_index_once_after:

``obox_refresh_index_once_after``
---------------------------------

- Default: ``0``
- Values: :ref:`uint`

This forces the next mailbox open after the specified UNIX timestamp to refresh locally cached indexes to see if other backends have modified the user's indexes simultaneously.

.. _plugin-obox-setting_obox_max_parallel_writes:

``obox_max_parallel_writes``
----------------------------

- Default: :ref:`setting-mail_prefetch_count`
- Values: :ref:`uint`

Maximum number of email write HTTP operations to do in parallel.


.. _plugin-obox-setting_obox_max_parallel_copies:

``obox_max_parallel_copies``
----------------------------

- Default: :ref:`setting-mail_prefetch_count`
- Values: :ref:`uint`

Maximum number of email HTTP copy/link operations to do in parallel.

If the storage driver supports bulk-copy/link operation, this controls how
many individual copy operations can be packed into a single bulk-copy/link
HTTP request.

.. _plugin-obox-setting_obox_max_parallel_deletes:

``obox_max_parallel_deletes``
-----------------------------

- Default: :ref:`setting-mail_prefetch_count`
- Values: :ref:`uint`

Maximum number of email HTTP delete operations to do in parallel.

If the storage driver supports bulk-delete operation, this controls how
many individual delete operations can be packed into a single bulk-delete
HTTP request.


.. _plugin-obox-setting_obox_rescan_mails_once_after:

``obox_rescan_mails_once_after``
--------------------------------

- Default: ``0``
- Values: :ref:`uint`

This forces the next mailbox open after the specified UNIX timestamp to rescan the mails to make sure there aren't any unindexed mails.


.. _plugin-obox-setting_obox_fs:

``obox_fs``
-----------

- Default: <empty>
- Values: :ref:`string`

This setting handles the basic Object Storage configuration, typically via the plugin block of 90-plugin.conf.


.. _plugin-obox-setting_obox_index_fs:

``obox_index_fs``
-----------------

- Default: Same as obox_fs
- Values: :ref:`string`

This setting handles the object storage configuration for index bundles.

.. WARNING:: obox_index_fs is currently not compatible with fs-posix driver.


.. _plugin-obox-setting_metacache_close_delay:

``metacache_close_delay``
-------------------------

- Default: ``2secs``
- Values: :ref:`time`

If user was accessed this recently, assume the user's indexes are up-to-date.
If not, list index bundles in object storage (or Cassandra) to see if they
have changed. This typically matters only when user is being moved to another
backend and soon back again, or if the user is simultaneously being accessed
by multiple backends. Default is 2 seconds.

Must be less than :ref:`setting-director_user_expire` (Default: 15min).


.. _plugin-obox-setting_metacache_max_space:

``metacache_max_space``
-----------------------

- Default: <empty>
- Values: :ref:`size`

How much disk space metacache can use before old data is cleaned up.

Generally, this should be set at ~90% of the available disk space.


.. _plugin-obox-setting_metacache_max_grace:

``metacache_max_grace``
-----------------------

- Default: 1G
- Values: :ref:`size`

How much disk space on top of metacache_max_space can be used before
Dovecot stops allowing more users to login.


.. _plugin-obox-setting_metacache_upload_interval:

``metacache_upload_interval``
-----------------------------

- Default: ``5min``
- Values: :ref:`time`

How often to upload important index changes to object storage? This mainly
means that if a backend crashes during this time, message flag changes within
this time may be lost. A longer time can however reduce the number of index
bundle uploads.

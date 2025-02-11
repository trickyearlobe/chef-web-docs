=====================================================
mount resource
=====================================================
`[edit on GitHub] <https://github.com/chef/chef-web-docs/blob/master/chef_master/source/resource_mount.rst>`__

Use the **mount** resource to manage a mounted file system.

Syntax
=====================================================
The mount resource has the following syntax:

.. code-block:: ruby

  mount 'name' do
    device           String
    device_type      String, Symbol # default value: :device
    domain           String
    dump             Integer, false # default value: 0
    enabled          true, false # default value: false
    fsck_device      String # default value: "-"
    fstype           String # default value: "auto"
    mount_point      String # default value: 'name' unless specified
    options          Array, String # default value: ["defaults"]
    pass             Integer, false # default value: 2
    password         String
    supports         Array, Hash
    username         String
    action           Symbol # defaults to :mount if not specified
  end

where:

* ``mount`` is the resource.
* ``name`` is the name given to the resource block.
* ``action`` identifies which steps Chef Infra Client will take to bring the node into the desired state.
* ``device``, ``device_type``, ``domain``, ``dump``, ``enabled``, ``fsck_device``, ``fstype``, ``mount_point``, ``options``, ``pass``, ``password``, ``supports``, and ``username`` are the properties available to this resource.

Actions
=====================================================

The mount resource has the following actions:

``:disable``
   Remove an entry from the file systems table (``fstab``).

``:enable``
   Add an entry to the file systems table (``fstab``).

``:mount``
   Default. Mount a device.

``:nothing``
   .. tag resources_common_actions_nothing

   This resource block does not act unless notified by another resource to take action. Once notified, this resource block either runs immediately or is queued up to run at the end of a Chef Infra Client run.

   .. end_tag

``:remount``
   Remount a device.

``:umount``
   Unmount a device.

``:unmount``
   Alias for ``:umount`` action.

.. note:: Order matters when passing multiple actions. For example: ``action [:mount, :enable]`` ensures that the file system is mounted before it is enabled.

Properties
=====================================================

The mount resource has the following properties:

``device``
   **Ruby Type:** String

   Required for ``:umount`` and ``:remount`` actions (for the purpose of checking the mount command output for presence). The special block device or remote node, a label, or a uuid to be mounted.

``device_type``
   **Ruby Type:** String, Symbol | **Default Value:** ``:device``

   The type of device: :device, :label, or :uuid


``domain``
   **Ruby Type:** String

   Windows only: Use to specify the domain in which the ``username`` and ``password`` are located.

``dump``
   **Ruby Type:** Integer, false | **Default Value:** ``0``

   The dump frequency (in days) used while creating a file systems table (fstab) entry.

``enabled``
   **Ruby Type:** true, false | **Default Value:** ``false``

   Use to specify if a mounted file system is enabled.

``fsck_device``
   **Ruby Type:** String | **Default Value:** ``"-"``

   Solaris only: The fsck device.


``fstype``
   **Ruby Type:** String | **Default Value:** ``"auto"``

   The file system type (fstype) of the device.

``mount_point``
   **Ruby Type:** String | **Default Value:** ``The resource block's name``

   The directory (or path) in which the device is to be mounted. Defaults to the name of the resource block if not provided.


``options``
   **Ruby Type:** Array, String | **Default Value:** ``["defaults"]``

   An array or comma separated list of options for the mount.

``pass``
   **Ruby Type:** Integer, false | **Default Value:** ``2``

   The pass number used by the file system check (``fsck``) command while creating a file systems table (``fstab``) entry.

``password``
   **Ruby Type:** String

   Windows only. Use to specify the password for ``username``.

``supports``
   **Ruby Type:** Array, Hash

   Specify a Hash of supported mount features. Default value: ``remount: false`` (preferred). Array defaults to ``remount: true`` (non-preferred).

``username``
   **Ruby Type:** String

   Windows only: Use to specify the user name.

Common Resource Functionality
=====================================================

Chef resources include common properties, notifications, and resource guards.

Common Properties
-----------------------------------------------------

.. tag resources_common_properties

The following properties are common to every resource:

``ignore_failure``
  **Ruby Type:** true, false | **Default Value:** ``false``

  Continue running a recipe if a resource fails for any reason.

``retries``
  **Ruby Type:** Integer | **Default Value:** ``0``

  The number of attempts to catch exceptions and retry the resource.

``retry_delay``
  **Ruby Type:** Integer | **Default Value:** ``2``

  The retry delay (in seconds).

``sensitive``
  **Ruby Type:** true, false | **Default Value:** ``false``

  Ensure that sensitive resource data is not logged by Chef Infra Client.

.. end_tag

Notifications
-----------------------------------------------------

``notifies``
  **Ruby Type:** Symbol, 'Chef::Resource[String]'

  .. tag resources_common_notification_notifies

  A resource may notify another resource to take action when its state changes. Specify a ``'resource[name]'``, the ``:action`` that resource should take, and then the ``:timer`` for that action. A resource may notify more than one resource; use a ``notifies`` statement for each resource to be notified.

  .. end_tag

.. tag resources_common_notification_timers

A timer specifies the point during a Chef Infra Client run at which a notification is run. The following timers are available:

``:before``
   Specifies that the action on a notified resource should be run before processing the resource block in which the notification is located.

``:delayed``
   Default. Specifies that a notification should be queued up, and then executed at the end of a Chef Infra Client run.

``:immediate``, ``:immediately``
   Specifies that a notification should be run immediately, per resource notified.

.. end_tag

.. tag resources_common_notification_notifies_syntax

The syntax for ``notifies`` is:

.. code-block:: ruby

  notifies :action, 'resource[name]', :timer

.. end_tag

``subscribes``
  **Ruby Type:** Symbol, 'Chef::Resource[String]'

.. tag resources_common_notification_subscribes

A resource may listen to another resource, and then take action if the state of the resource being listened to changes. Specify a ``'resource[name]'``, the ``:action`` to be taken, and then the ``:timer`` for that action.

Note that ``subscribes`` does not apply the specified action to the resource that it listens to - for example:

.. code-block:: ruby

 file '/etc/nginx/ssl/example.crt' do
   mode '0600'
   owner 'root'
 end

 service 'nginx' do
   subscribes :reload, 'file[/etc/nginx/ssl/example.crt]', :immediately
 end

In this case the ``subscribes`` property reloads the ``nginx`` service whenever its certificate file, located under ``/etc/nginx/ssl/example.crt``, is updated. ``subscribes`` does not make any changes to the certificate file itself, it merely listens for a change to the file, and executes the ``:reload`` action for its resource (in this example ``nginx``) when a change is detected.

.. end_tag

.. tag resources_common_notification_timers

A timer specifies the point during a Chef Infra Client run at which a notification is run. The following timers are available:

``:before``
   Specifies that the action on a notified resource should be run before processing the resource block in which the notification is located.

``:delayed``
   Default. Specifies that a notification should be queued up, and then executed at the end of a Chef Infra Client run.

``:immediate``, ``:immediately``
   Specifies that a notification should be run immediately, per resource notified.

.. end_tag

.. tag resources_common_notification_subscribes_syntax

The syntax for ``subscribes`` is:

.. code-block:: ruby

   subscribes :action, 'resource[name]', :timer

.. end_tag

Guards
-----------------------------------------------------

.. tag resources_common_guards

A guard property can be used to evaluate the state of a node during the execution phase of a Chef Infra Client run. Based on the results of this evaluation, a guard property is then used to tell Chef Infra Client if it should continue executing a resource. A guard property accepts either a string value or a Ruby block value:

* A string is executed as a shell command. If the command returns ``0``, the guard is applied. If the command returns any other value, then the guard property is not applied. String guards in a **powershell_script** run Windows PowerShell commands and may return ``true`` in addition to ``0``.
* A block is executed as Ruby code that must return either ``true`` or ``false``. If the block returns ``true``, the guard property is applied. If the block returns ``false``, the guard property is not applied.

A guard property is useful for ensuring that a resource is idempotent by allowing that resource to test for the desired state as it is being executed, and then if the desired state is present, for Chef Infra Client to do nothing.

.. end_tag

.. tag resources_common_guards_properties

The following properties can be used to define a guard that is evaluated during the execution phase of a Chef Infra Client run:

``not_if``
  Prevent a resource from executing when the condition returns ``true``.

``only_if``
  Allow a resource to execute only if the condition returns ``true``.

.. end_tag

Examples
=====================================================
The following examples demonstrate various approaches for using resources in recipes:

**Mount a labeled file system**

.. tag resource_mount_labeled_file_system

.. To mount a labeled file system:

.. code-block:: ruby

   mount '/mnt/volume1' do
     device 'volume1'
     device_type :label
     fstype 'xfs'
     options 'rw'
   end

.. end_tag

**Mount a local block drive**

.. tag resource_mount_local_block_device

.. To mount a local block device:

.. code-block:: ruby

   mount '/mnt/local' do
     device '/dev/sdb1'
     fstype 'ext3'
   end

.. end_tag

**Mount a non-block file system**

.. tag resource_mount_nonblock_file_system

.. To mount a non-block file system

.. code-block:: ruby

   mount '/mount/tmp' do
     pass     0
     fstype   'tmpfs'
     device   '/dev/null'
     options  'nr_inodes=999k,mode=755,size=500m'
     action   [:mount, :enable]
   end

.. end_tag

**Mount and add to the file systems table**

.. tag resource_mount_remote_file_system_add_to_fstab

.. To mount a remote file system and add it to the file systems table:

.. code-block:: ruby

   mount '/export/www' do
     device 'nas1prod:/export/web_sites'
     fstype 'nfs'
     options 'rw'
     action [:mount, :enable]
   end

.. end_tag

**Mount a remote file system**

.. tag resource_mount_remote_file_system

.. To mount a remote file system:

.. code-block:: ruby

   mount '/export/www' do
     device 'nas1prod:/export/web_sites'
     fstype 'nfs'
     options 'rw'
   end

.. end_tag

**Mount a remote folder in Microsoft Windows**

.. tag resource_mount_remote_windows_folder

.. To mount a remote Microsoft Windows folder on local drive letter T:

.. code-block:: ruby

   mount 'T:' do
     action :mount
     device '\\\\hostname.example.com\\folder'
   end

.. end_tag

**Unmount a remote folder in Microsoft Windows**

.. tag resource_mount_unmount_remote_windows_drive

.. To un-mount a remote Microsoft Windows D: drive attached as local drive letter T:

.. code-block:: ruby

   mount 'T:' do
     action :umount
     device '\\\\hostname.example.com\\D$'
   end

.. end_tag

**Stop a service, do stuff, and then restart it**

.. tag resource_service_stop_do_stuff_start

The following example shows how to use the **execute**, **service**, and **mount** resources together to ensure that a node running on Amazon EC2 is running MySQL. This example does the following:

* Checks to see if the Amazon EC2 node has MySQL
* If the node has MySQL, stops MySQL
* Installs MySQL
* Mounts the node
* Restarts MySQL

.. code-block:: ruby

   # the following code sample comes from the ``server_ec2``
   # recipe in the following cookbook:
   # https://github.com/chef-cookbooks/mysql

   if (node.attribute?('ec2') && ! FileTest.directory?(node['mysql']['ec2_path']))

     service 'mysql' do
       action :stop
     end

     execute 'install-mysql' do
       command "mv #{node['mysql']['data_dir']} #{node['mysql']['ec2_path']}"
       not_if do FileTest.directory?(node['mysql']['ec2_path']) end
     end

     [node['mysql']['ec2_path'], node['mysql']['data_dir']].each do |dir|
       directory dir do
         owner 'mysql'
         group 'mysql'
       end
     end

     mount node['mysql']['data_dir'] do
       device node['mysql']['ec2_path']
       fstype 'none'
       options 'bind,rw'
       action [:mount, :enable]
     end

     service 'mysql' do
       action :start
     end

   end

where

* the two **service** resources are used to stop, and then restart the MySQL service
* the **execute** resource is used to install MySQL
* the **mount** resource is used to mount the node and enable MySQL

.. end_tag

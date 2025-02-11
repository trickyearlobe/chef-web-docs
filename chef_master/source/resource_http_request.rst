=====================================================
http_request resource
=====================================================
`[edit on GitHub] <https://github.com/chef/chef-web-docs/blob/master/chef_master/source/resource_http_request.rst>`__

.. tag resource_http_request_summary

Use the **http_request** resource to send an HTTP request (``GET``, ``PUT``, ``POST``, ``DELETE``, ``HEAD``, or ``OPTIONS``) with an arbitrary message. This resource is often useful when custom callbacks are necessary.

.. end_tag

Syntax
=====================================================
A **http_request** resource block sends HTTP requests with an arbitrary message. For example, send a ``DELETE`` request to ``'http://www.chef.io/some_page?message=please_delete_me'``.

.. code-block:: ruby

   http_request 'please_delete_me' do
     url 'http://www.chef.io/some_page'
     action :delete
   end

The full syntax for all of the properties that are available to the **http_request** resource is:

.. code-block:: ruby

   http_request 'name' do
     headers                    Hash
     message                    Object # defaults to 'name' if not specified
     url                        String
     action                     Symbol # defaults to :get if not specified
   end

where:

* ``http_request`` is the resource.
* ``name`` is the name given to the resource block.
* ``action`` identifies which steps Chef Infra Client will take to bring the node into the desired state.
* ``headers``, ``message``, and ``url`` are properties of this resource, with the Ruby type shown. See "Properties" section below for more information about all of the properties that may be used with this resource.

Actions
=====================================================

The http_request resource has the following actions:

``:delete``
   Send a ``DELETE`` request.

``:get``
   Default. Send a ``GET`` request.

   Changed in Chef Client 12.0 to deprecate the hard-coded query string from earlier versions. Cookbooks that rely on this string need to be updated to manually add it to the URL as it is passed to the resource.

``:head``
   Send a ``HEAD`` request.

``:nothing``
   .. tag resources_common_actions_nothing

   This resource block does not act unless notified by another resource to take action. Once notified, this resource block either runs immediately or is queued up to run at the end of a Chef Infra Client run.

   .. end_tag

``:options``
   Send an ``OPTIONS`` request.

``:post``
   Send a ``POST`` request.

``:put``
   Send a ``PUT`` request.

Properties
=====================================================

The http_request resource has the following properties:

``headers``
   **Ruby Type:** Hash

   A Hash of custom headers.

``message``
   **Ruby Type:** Object

   The message that is sent by the HTTP request. Default value: the ``name`` of the resource block. See "Syntax" section above for more information.

``url``
   **Ruby Type:** String

   The URL to which an HTTP request is sent.

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

**Send a GET request**

.. tag resource_http_request_send_get

.. To send a GET request:

.. code-block:: ruby

   http_request 'some_message' do
     url 'http://example.com/check_in'
   end

The message is sent as ``http://example.com/check_in?message=some_message``.

.. end_tag

**Send a POST request**

.. tag resource_http_request_send_post

To send a ``POST`` request as JSON data, convert the message to JSON and include the correct content-type header. For example:

.. code-block:: ruby

   http_request 'posting data' do
     action :post
     url 'http://example.com/check_in'
     message ({:some => 'data'}.to_json)
     headers({'AUTHORIZATION' => "Basic #{
       Base64.encode64('username:password')}",
       'Content-Type' => 'application/data'
     })
   end

.. end_tag

**Transfer a file only when the remote source changes**

.. tag resource_remote_file_transfer_remote_source_changes

.. To transfer a file only if the remote source has changed (using the |resource http request| resource):

.. The "Transfer a file only when the source has changed" example is deprecated in Chef Client 11.6

.. code-block:: ruby

   remote_file '/tmp/couch.png' do
     source 'http://couchdb.apache.org/img/sketch.png'
     action :nothing
   end

   http_request 'HEAD http://couchdb.apache.org/img/sketch.png' do
     message ''
     url 'http://couchdb.apache.org/img/sketch.png'
     action :head
     if ::File.exist?('/tmp/couch.png')
       headers 'If-Modified-Since' => File.mtime('/tmp/couch.png').httpdate
     end
     notifies :create, 'remote_file[/tmp/couch.png]', :immediately
   end

.. end_tag

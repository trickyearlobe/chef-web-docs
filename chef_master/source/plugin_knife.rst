=====================================================
About Knife Cloud Plugins
=====================================================
`[edit on GitHub] <https://github.com/chef/chef-web-docs/blob/master/chef_master/source/plugin_knife.rst>`__

.. tag plugin_knife_summary

A knife plugin is a set of one (or more) subcommands that can be added to knife to support additional functionality that is not built-in to the base set of knife subcommands. Many of the knife plugins are built by members of the Chef community and several of them are built and maintained by Chef.

.. end_tag

* The same `common options </knife_options.html>`__ used by knife subcommands can also be used by knife plug-ins
* A knife plugin can make authenticated API requests to the Chef server

Build a `custom plugin </plugin_knife_custom.html>`__ or use one of the following plugins that are maintained by Chef:

The following knife plug-ins are maintained by Chef:

.. list-table::
   :widths: 150 450
   :header-rows: 1

   * - Subcommand
     - Description
   * - `knife-azure <https://github.com/chef/knife-azure>`__
     - .. tag knife_azure

       Microsoft Azure is a cloud hosting platform from Microsoft that provides virtual machines for Linux and Windows Server, cloud and database services, and more. Use the ``knife azure`` subcommand to manage API-driven cloud servers hosted by Microsoft Azure.

       .. end_tag

   * - `knife-ec2 <https://github.com/chef/knife-ec2>`__
     - Amazon EC2 is a web service that provides resizable compute capacity in the cloud, based on preconfigured operating systems and virtual application software using Amazon Machine Images (AMI). The ``knife ec2`` subcommand is used to manage API-driven cloud servers that are hosted by Amazon EC2.
   * - `knife-google <https://github.com/chef/knife-google>`__
     - Google Compute Engine is a cloud hosting platform that offers scalable and flexible virtual machine computing. The ``knife google`` subcommand is used to manage API-driven cloud servers that are hosted by Google Compute Engine.
   * - `knife-openstack <https://github.com/chef/knife-openstack>`__
     - The ``knife openstack`` subcommand is used to manage API-driven cloud servers that are hosted by OpenStack.
   * - `knife-rackspace <https://github.com/chef/knife-rackspace>`__
     - Rackspace is a cloud-driven platform of virtualized servers that provide services for storage and data, platform and networking, and cloud computing. The ``knife rackspace`` subcommand is used to manage API-driven cloud servers that are hosted by Rackspace cloud services
   * - `knife-vcenter <https://github.com/chef/knife-vcenter>`__
     - The ``knife vcenter`` subcommand is used to provision systems with VMware vCenter.
   * - `knife-vsphere <https://github.com/chef/knife-vsphere>`__
     - The ``knife vsphere`` subcommand is used to provision systems with VMware vSphere.

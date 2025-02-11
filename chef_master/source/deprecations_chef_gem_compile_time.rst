=====================================================
Deprecation: Chef Gem Compile Time (CHEF-3)
=====================================================
`[edit on GitHub] <https://github.com/chef/chef-web-docs/blob/master/chef_master/source/deprecations_chef_gem_compile_time.rst>`__

Originally, the `chef gem </resource_chef_gem.html>`__ resource always ran during the `compile` phase (see this section on `Chef Infra Client runs </chef_client_overview.html#the-chef-client-run>`__ for further details).
It is now possible to control which phase the resource is run in. Calling ``chef_gem`` without specifying the phase is now deprecated.



This deprecation warning was added in Chef Client 12.1.0, and using ``chef_gem`` without specifying a phase will become an error in Chef Client 13.

Remediation
================

There are two possible remediations.

The first is to set the ``compile_time`` property on the resource. To maintain the same behavior as before, the property should be set to ``true``:

.. code-block:: ruby

  chef_gem "etcd" do
    compile_time true
  end

The second, and preferred, is to add a `gem dependency </config_rb_metadata.html>`__ in your cookbook metadata.

.. code-block:: ruby

  gem "etcd"

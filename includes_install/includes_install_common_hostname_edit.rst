.. This is an included how-to. 


In some cases, the hostname for the |chef server| needs to be updated. The process for updating a hostname varies, depending on the platform on which the |chef server| will run. Refer to the manual for the platform or contact a local systems administrator for specific guidance for a specific platform. The following example shows how a hostname can be changed when running |redhat| or |centos|:

.. code-block:: bash

   $ sudo hostname 'mychefserver.example.com'

and then:

.. code-block:: bash

   $ echo "mychefserver.example.com" | sudo tee /etc/hostname
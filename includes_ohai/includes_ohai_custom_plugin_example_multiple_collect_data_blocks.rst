.. The contents of this file are included in multiple topics.
.. This file should not be changed in a way that hinders its ability to appear in multiple documentation sets.


The following |ohai| plugin uses multiple ``collect_data`` blocks and shared methods to define platforms:

.. code-block:: ruby

   Ohai.plugin(:Hostname) do
     provides 'domain', 'fqdn', 'hostname'
   
     def from_cmd(cmd)
       so = shell_out(cmd)
       so.stdout.split($/)[0]
     end
   
     def collect_domain
       if fqdn
         fqdn =~ /.+?\.(.*)/
         domain $1
       end
     end
   
     collect_data(:aix, :hpux) do
       hostname from_cmd('hostname -s')
       fqdn from_cmd('hostname')
       domain collect_domain
     end
   
     collect_data(:darwin, :netbsd, :openbsd) do
       hostname from_cmd('hostname -s')
       fqdn from_cmd('hostname')
       domain collect_domain
     end
   
     collect_data(:freebsd) do
       hostname from_cmd('hostname -s')
       fqdn from_cmd('hostname -f')
       domain collect_domain
     end
   
     collect_data(:linux) do
       hostname from_cmd('hostname -s')
       begin
         fqdn from_cmd('hostname --fqdn')
       rescue
         Ohai::Log.debug('hostname -f returned an error, probably no domain is set')
       end
       domain collect_domain
     end
   
     collect_data(:solaris2) do
       require 'socket'
   
       hostname from_cmd('hostname')
   
       fqdn_lookup = Socket.getaddrinfo(hostname, nil, nil, nil, nil, Socket::AI_CANONNAME).first[2]
       if fqdn_lookup.split('.').length > 1
         # we received an fqdn
         fqdn fqdn_lookup
       else
         # default to assembling one
         h = from_cmd('hostname')
         d = from_cmd('domainname')
         fqdn '#{h}.#{d}'
       end
   
       domain collect_domain
     end
  
     collect_data(:windows) do
       require 'ruby-wmi'
       require 'socket'
   
       host = WMI::Win32_ComputerSystem.find(:first)
       hostname '#{host.Name}' 
   
       info = Socket.gethostbyname(Socket.gethostname)
       if info.first =~ /.+?\.(.*)/
         fqdn info.first
       else
         # host is not in dns. optionally use:
         # C:\WINDOWS\system32\drivers\etc\hosts
         fqdn Socket.gethostbyaddr(info.last).first
       end
   
      domain collect_domain
     end
   end

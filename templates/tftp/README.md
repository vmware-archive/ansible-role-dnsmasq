# ansible-role-dnsmasq/templates/tftp

This directory would have a set of files (templates) that create the various
tftp and web root directory (webroot) files necessary to support boot services.

## Required Files

This directory does not need files, rather you would place the relevant files
in your <playbook>/tftp directory, but they nature of the requite files is
discussed here.

    webroot.j2

The webroot.j2 template creates the tftp webroot tftp index page. This file
will be created at /tftp/webroot/index.html (the root of the tftp web site).

   default.j2

The default.j2 template creates the PXE boot configuration file. This will
be created at /tftp/pxeboot/pxelinux.cfg/default.

   pxe.conf.j2

The pxe.conf.j2 template create the PXE boot menu. This file will be created
at /tftp/pxeboot/pxelinux.cfg/pxe.conf.

   interactive.menu.j2

The interactive.menu.j2 templates creates an interactive menu (assumed to be
so anyway) menu. This file is created at
/tftp/pxeboot/pxelinux.cfg/conf/Interactive.menu.

   scripted.menu.j2

The scripted.menu.j2 template creates a scripted (assumed to be so anyway)
boot configuration file. This file gets created in
/tftp/pxeboot/pxelinux.cfg/conf/Scripted.menu.

For a sample setup, see
[Chaperone tftp setup](https://github.com/vmware/ansible-playbooks-chaperone/tree/master/chaperone-ui/tftp).

Ansible TPM Simulator
=====================

This ansible role deploys CentOS 7.5 and the TPM2 tools listed below, alongside
IBM's Software TPM 2.0 developed by Ken Goldman.

The idea is that this will allow developers to quickly bring up a virtual
environment to learn the TPM2 tool set and start to develop their own TPM
applications.

Vagrant file currently works with libvirt and VirtualBox as providers.

Tools Included
--------------

* tpm2_tools
* tpm2-abrmd
* tpm2-tss

Other supporting software

* tboot
* openssl-devel

rng-tools randomness
--------------------

When using `libvirt` as a provider, a virtio feed is made from the hosts
/dev/urandom to the guest, in order to help with entropy needs.

Usage
=====

Clone the repository and then simply run `vargant up --provider <provider>`.

For example, using libvirt:

`vagrant up --provider libvirt`

For example, using VirtualBox:

`vagrant up --provider virtualbox`

Once the VM is started, `vagrant ssh` into the VM and run `sudo su -` to become
root.

From here navigate to `/root/ibmtpm974/src` and run `./tpm_server &`. For
example:

    [root@tpm2-simulator src]# ./tpm_server
    TPM command server listening on port 2321
    Platform server listening on port 2322

You can now start the resource manager daemon as follows:

    [root@tpm2-simulator src]# tpm2-abrmd -t socket
    Client accepted

If you prefer to use systemd to manage the resource manager , just amend
the file `/usr/lib/systemd/system/tpm2-abrmd.service` as follows::

    [Service]
    Type=dbus
    Restart=always
    RestartSec=5
    EnvironmentFile=-/etc/default/tpm2-abrmd
    BusName=com.intel.tss2.Tabrmd
    StandardOutput=syslog
    ExecStart=/usr/sbin/tpm2-abrmd -t socket
    User=tss

    [Install]
    WantedBy=multi-user.target

Namely add `-t socker` to the `ExecStart` line.

Basic Commands
==============

For full list of commands, consult the tpm2-tools documentation or consult the
man pages for each command, here are some examples though to get you started.

Take ownership
--------------

To take ownership of the (software) TPM:

    tpm2_takeownership -o ownerpass -e endorsepass -l lockpass

This will set passwords for the owner, endorsement and lockout.

List Current PCR Values
-----------------------

    tpm2_pcrlist

Error Codes
-----------

Error codes can be viewed with `tpm2_rc_decode`, for example:

```
$ tpm2_rc_decode 0x921
error layer
  hex: 0x0
  identifier: TSS2_TPM_ERROR_LEVEL
  description: Error produced by the TPM
format 0 warning code
  hex: 0x21
  name: TPM_RC_LOCKOUT
  description: authorizations for objects subject to DA protection are not allowed at this time because the TPM is in DA lockout mode
```

License
-------

Apache 2.0

Contribute
----------

Please do! Pull requests are welcome as I have not done a great deal here yet!

Author Information
------------------

Luke Hinds (lhinds@redhat.com)

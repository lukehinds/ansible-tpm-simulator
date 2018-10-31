Ansible TPM Simulator
=====================

This ansible role deploys CentOS 7.5 and builds / installs TPM2 software listed
below, alongside IBM's Software TPM 2.0 developed by Ken Goldman.

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

You can then start the TPM simulator as follows:

    [root@tpm2-simulator ~]# tpm_server
    TPM command server listening on port 2321
    Platform server listening on port 2322

* Tip: use `tpm_server -c` to clear ownership.

You can enable / start the resource manager as a systemd service as follows:

    [root@tpm2-simulator src]# systemctl enable tpm2-abrmd

    [root@tpm2-simulator src]# systemctl start tpm2-abrmd

TPM2 Software Branches
======================

You can set branch names that will be used for building using the following
vars set within `roles/tpm2-simulator/vars/main.yml`

    tpm2_tss_version: 2.1.0
    tpm2_abrmd_version: 2.0.2
    tpm2_tools_version: v1.1.0

Configure Arguments
===================

Configure arguments can be set within `roles/tpm2-simulator/vars/main.yml`, for
example:

    tpm2_tss_configure_args: "./configure --prefix=/usr --enable-debug --with-crypto=ossl --disable-doxygen-doc"
    tpm2_abrmd_configure_args: "TSS2_SYS_CFLAGS=' ' TSS2_SYS_LIBS='-ltss2-sys -L/usr/lib/' ./configure --prefix=/usr --with-dbuspolicydir=/etc/dbus-1/system.d"
    tpm2_tools_configure_args: "SAPI_CFLAGS=' ' SAPI_LIBS='-ltss2-sys -L/usr/lib/' ./configure --prefix=/usr"

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

Please do! Pull requests are welcome.

Author Information
------------------

Luke Hinds (lhinds@redhat.com)

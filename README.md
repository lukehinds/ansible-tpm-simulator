Ansible TPM Simulator
=====================

This ansible role deploys CentOS 7.5 and the TPM2 tools listed below, alongside
IBM's Software TPM 2.0 developed by Ken Goldman.

The idea is that this will allow developers to quickly bring up a virtual
environment to learn the TPM2 tool set and start to develop their own TPM
applications.

Tools Included
--------------

* tpm2_tools
* tpm2-abrmd
* tpm2-tss

Other supporting software

* tboot
* openssl-devel

Usage
=====

Clone the repository and then simply run `vargant up`.

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


License
-------

Apache 2.0

Author Information
------------------

Luke Hinds (lhinds@redhat.com)

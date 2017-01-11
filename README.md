# se_test.yml

**A failed task/play means a test did not succeed.**

Do you have a set of manual tests you run to check that the basic functionality of your storageelement is working after some maintenance? Please let me know what you test - maybe we can add it here. 

An ansible playbook to test a storage element.

 - listing
 - writing
 - reading
 - checksum comparison of written and fetched files
 - test that some Glue data is published in BDII

## dependencies

 - nordugrid-arc-client
  - arcproxy, arcls and arccp
 - xrootd-client
  - xrdfs and xrdcp
 - openldap-clients (or ldap-utils)
  - ldapsearch

## requirements

 - a storage element(only tested with dCache)
  - with an SRM and an xrootd door
   - for SRM a place to write would be cool, xrdcp then copies the file
 - a proxy certificate

## usage

<pre>
ansible-playbook se_test.yml -e server=myhost.example.com -e path=/path/to/directory/in/namespace/where/I/haz/write/FOO -e file=BARZ -vv
</pre>
or
<pre>
ansible-playbook se_test.yml -e server=myhost.example.com -e path=/path/to/directory/in/namespace/where/I/haz/write/FOO -e file=BARZ -e xfed_server1=xrootdfederationhost1.example.com -e xfed_server2=xrootdfederationhost2.example.com -e xfed_pathfile=//store/user/myuser/AAAAAAAAAAAAA-AAAA-AAAA-AAAA-NOTREAL -e enable_ldap_testin=True
</pre>

## TODO:

 - use https://github.com/quinot/ansible-plugin-lookup_ldap to test ldap?
 - should the whole play fail if a test/task fail or is it enough that it at the end has

<pre>
PLAY RECAP *********************************************************************
localhost                  : ok=17   changed=11   unreachable=0    failed=1   
</pre>

## Example
<pre>

$ ansible-playbook se_test.yml -e server=myhost.example.com -e path=/path/to/directory/in/namespace/where/I/haz/write/FOO -e file=BARZ -e xfed_server1=xrootdfederationhost1.example.com -e xfed_server2=xrootdfederationhost2.example.com -e xfed_pathfile=//store/user/myuser/AAAAAAAAAAAAA-AAAA-AAAA-AAAA-NOTREAL -e enable_ldap_testin=True
 [WARNING]: Host file not found: /etc/ansible/hosts

 [WARNING]: provided hosts list is empty, only localhost is available


PLAY [test a storage element] **************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [findproxy validity - fails if no (valid) proxy found] ********************
ok: [localhost -> localhost]

TASK [arcls SRM dir] ***********************************************************
changed: [localhost -> localhost]

TASK [arcls SRM path / file] ***************************************************
changed: [localhost -> localhost]

TASK [arcrm SRM file first if it exists] ***************************************
changed: [localhost -> localhost]

TASK [xrdfs ls] ****************************************************************
changed: [localhost -> localhost]

TASK [stat bash] ***************************************************************
ok: [localhost -> localhost]

TASK [store bash checksum in a fact] *******************************************
ok: [localhost -> localhost]

TASK [arccp SRM here to there] *************************************************
changed: [localhost -> localhost]

TASK [arccp SRM there to here] *************************************************
changed: [localhost -> localhost]

TASK [xrdcp there to here] *****************************************************
changed: [localhost -> localhost]

TASK [stat fetched bash file] **************************************************
ok: [localhost -> localhost]

TASK [stat fetched bash file2] *************************************************
ok: [localhost -> localhost]

TASK [debug print checksums of fetched files] **********************************
skipping: [localhost] => (item=reg_bash_1) 
skipping: [localhost] => (item=reg_bash_2) 

TASK [assert that checksums are all the same] **********************************
ok: [localhost -> localhost] => (item=reg_bash_1) => {
    "changed": false, 
    "item": "reg_bash_1", 
    "msg": "All assertions passed"
}
ok: [localhost -> localhost] => (item=reg_bash_2) => {
    "changed": false, 
    "item": "reg_bash_2", 
    "msg": "All assertions passed"
}

TASK [xrdcp from xrootd federation server1 to here] ****************************
changed: [localhost -> localhost]

TASK [xrdcp from xrootd federation server2 to here] ****************************
changed: [localhost -> localhost]

TASK [xrdcp from xrootd federation server3 to here] ****************************
changed: [localhost -> localhost]

TASK [ldapsearch tool comes from openldap-clients on redhat] *******************
changed: [localhost -> localhost]
 [WARNING]: Consider using yum, dnf or zypper module rather than running rpm


TASK [ldapsearch tool comes from ldap-utils on debian] *************************
skipping: [localhost]

TASK [shell ldapsearch towards server port 2170 and grep for GlueSESizeTotal] **
ok: [localhost -> localhost]

TASK [debug print ldapsearch] **************************************************
skipping: [localhost]

RUNNING HANDLER [cleanup] ******************************************************
changed: [localhost]

RUNNING HANDLER [cleanup2] *****************************************************
changed: [localhost]

RUNNING HANDLER [cleanup3] *****************************************************
changed: [localhost] => (item=2)
changed: [localhost] => (item=3)
changed: [localhost] => (item=4)
changed: [localhost] => (item=5)

PLAY RECAP *********************************************************************
localhost                  : ok=22   changed=14   unreachable=0    failed=0   

</pre>

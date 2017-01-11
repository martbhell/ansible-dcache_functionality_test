# se_test.yml

Do you have a set of manual tests you run to check that the basic functionality of your storageelement is working after some maintenance? Please let me know what you test - maybe we can add it here. 

An ansible playbook to test a storage element.

 - listing
 - writing
 - reading
 - checksum comparison of written and fetched files

Also possible to test BDII

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

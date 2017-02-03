# se_test.yml

**A failed task/play means a test did not succeed.**

Do you have a set of manual tests you run to check that the basic functionality of your storageelement is working after some maintenance? Please let me know what you test - maybe we can add it here. 

An ansible playbook to test a storage element.

 - listing
 - writing
 - reading
 - checksum comparison of written and fetched files
 - test that some Glue data is published in BDII

It assumes a few things (for example that srm/bdii/xrootd are all reacahable from the same SRM and are on default ports), but that and in general most things should be configurable.

## dependencies

 - nordugrid-arc-client
  - arcproxy, arcls and arccp
 - xrootd-client
  - xrdfs and xrdcp
 - openldap-clients (or ldap-utils)
  - ldapsearch
 - gfal2-client (if enable_gfal2_testing: True)
  - utils and plugins

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
ansible-playbook se_test.yml -e server=myhost.example.com -e path=/path/to/directory/in/namespace/where/I/haz/write/FOO -e file=BARZ -e xfed_server1=xrootdfederationhost1.example.com -e xfed_server2=xrootdfederationhost2.example.com -e xfed_pathfile=//store/user/myuser/AAAAAAAAAAAAA-AAAA-AAAA-AAAA-NOTREAL -e enable_ldap_testin=True -e xrootd_server=notmyhost.example.com -e ldap_server=alsonotmyhost.example.com -e srm_server=yetanothernotmyhost.example.com
</pre>

## TODO:

 - use https://github.com/quinot/ansible-plugin-lookup_ldap to test ldap?
 - should the whole play fail if a test/task fail or is it enough that it at the end has

## Example output

With this command:
<pre>
ansible-playbook se_test.yml -e server=se.example.com -e path=/pnfs/csc.fi/data/cms/FOO -e file=BARZ -e enable_ldap_testing=True -e source_file=/bin/bash -vv
</pre>

Get an output like this:
<pre>

Using /home/myusername/git/ansible-dcache_functionality_test/ansible.cfg as config file
 [WARNING]: Host file not found: /etc/ansible/hosts

 [WARNING]: provided hosts list is empty, only localhost is available


PLAYBOOK: se_test.yml **********************************************************
1 plays in se_test.yml

PLAY [test a storage element] **************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [findproxy validity - fails if no (valid) proxy found] ********************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:60
ok: [localhost -> localhost] => {"changed": false, "cmd": ["arcproxy", "-i", "validityLeft"], "delta": "0:00:00.021785", "end": "2017-02-03 14:42:41.083646", "failed": false, "failed_when_result": false, "rc": 0, "start": "2017-02-03 14:42:41.061861", "stderr": "", "stdout": "22793", "stdout_lines": ["22793"], "warnings": []}

TASK [command rpm check if the gfal2_rpms are installed] ***********************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:69
ok: [localhost -> localhost] => (item=util) => {"changed": false, "cmd": ["rpm", "-q", "gfal2-util"], "delta": "0:00:00.039740", "end": "2017-02-03 14:42:41.461800", "item": "util", "rc": 0, "start": "2017-02-03 14:42:41.422060", "stderr": "", "stdout": "gfal2-util-1.4.0-1.el7.noarch", "stdout_lines": ["gfal2-util-1.4.0-1.el7.noarch"], "warnings": ["Consider using yum, dnf or zypper module rather than running rpm"]}
ok: [localhost -> localhost] => (item=plugin-srm) => {"changed": false, "cmd": ["rpm", "-q", "gfal2-plugin-srm"], "delta": "0:00:00.047165", "end": "2017-02-03 14:42:41.716802", "item": "plugin-srm", "rc": 0, "start": "2017-02-03 14:42:41.669637", "stderr": "", "stdout": "gfal2-plugin-srm-2.12.3-2.el7.x86_64", "stdout_lines": ["gfal2-plugin-srm-2.12.3-2.el7.x86_64"], "warnings": ["Consider using yum, dnf or zypper module rather than running rpm"]}
ok: [localhost -> localhost] => (item=plugin-xrootd) => {"changed": false, "cmd": ["rpm", "-q", "gfal2-plugin-xrootd"], "delta": "0:00:00.050258", "end": "2017-02-03 14:42:41.980707", "item": "plugin-xrootd", "rc": 0, "start": "2017-02-03 14:42:41.930449", "stderr": "", "stdout": "gfal2-plugin-xrootd-2.12.3-2.el7.x86_64", "stdout_lines": ["gfal2-plugin-xrootd-2.12.3-2.el7.x86_64"], "warnings": ["Consider using yum, dnf or zypper module rather than running rpm"]}
ok: [localhost -> localhost] => (item=plugin-gridftp) => {"changed": false, "cmd": ["rpm", "-q", "gfal2-plugin-gridftp"], "delta": "0:00:00.031862", "end": "2017-02-03 14:42:42.223023", "item": "plugin-gridftp", "rc": 0, "start": "2017-02-03 14:42:42.191161", "stderr": "", "stdout": "gfal2-plugin-gridftp-2.12.3-2.el7.x86_64", "stdout_lines": ["gfal2-plugin-gridftp-2.12.3-2.el7.x86_64"], "warnings": ["Consider using yum, dnf or zypper module rather than running rpm"]}
ok: [localhost -> localhost] => (item=plugin-http) => {"changed": false, "cmd": ["rpm", "-q", "gfal2-plugin-http"], "delta": "0:00:00.025071", "end": "2017-02-03 14:42:42.441638", "item": "plugin-http", "rc": 0, "start": "2017-02-03 14:42:42.416567", "stderr": "", "stdout": "gfal2-plugin-http-2.12.3-2.el7.x86_64", "stdout_lines": ["gfal2-plugin-http-2.12.3-2.el7.x86_64"], "warnings": ["Consider using yum, dnf or zypper module rather than running rpm"]}
ok: [localhost -> localhost] => (item=plugin-file) => {"changed": false, "cmd": ["rpm", "-q", "gfal2-plugin-file"], "delta": "0:00:00.051480", "end": "2017-02-03 14:42:42.708681", "item": "plugin-file", "rc": 0, "start": "2017-02-03 14:42:42.657201", "stderr": "", "stdout": "gfal2-plugin-file-2.12.3-2.el7.x86_64", "stdout_lines": ["gfal2-plugin-file-2.12.3-2.el7.x86_64"], "warnings": ["Consider using yum, dnf or zypper module rather than running rpm"]}

TASK [if gfal2-util is installed test with that too] ***************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:77
ok: [localhost -> localhost] => {"ansible_facts": {"ls_program": ["arcls", "gfal-ls"], "xrootd_ls_program": ["xrdfs", "gfal-ls"]}, "changed": false}

TASK [otherwise just use arcls or xrdfs] ***************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:83
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [arcls SRM dir] ***********************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:91
changed: [localhost -> localhost] => (item=arcls) => {"changed": true, "cmd": ["arcls", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO"], "delta": "0:00:02.168945", "end": "2017-02-03 14:42:45.188476", "item": "arcls", "rc": 0, "start": "2017-02-03 14:42:43.019531", "stderr": "", "stdout": "A\nA1\nA2\nA3\nA4\nA5\nA6\nB\nBAR\nBARZ4\nBARZ6\nC1\nC2\nC3\nC4\nC5\nC6\nC7\nC8\nD1\nD2\nD3\nD4\nD5\nD6\nD7\nE1\nE3\nF1\nF2\nF3\nF4\nG1\nG2\nH1\nH10\nH2\nH3\nH4\nH5\nH6\nH8\nH9\nI1\nI10\nI11\nI12\nI13\nI14\nI15\nI16\nI2\nI3\nI4\nI5\nI6\nI7\nI8\nI9\ntjena\ntjena2\ntjena3", "stdout_lines": ["A", "A1", "A2", "A3", "A4", "A5", "A6", "B", "BAR", "BARZ4", "BARZ6", "C1", "C2", "C3", "C4", "C5", "C6", "C7", "C8", "D1", "D2", "D3", "D4", "D5", "D6", "D7", "E1", "E3", "F1", "F2", "F3", "F4", "G1", "G2", "H1", "H10", "H2", "H3", "H4", "H5", "H6", "H8", "H9", "I1", "I10", "I11", "I12", "I13", "I14", "I15", "I16", "I2", "I3", "I4", "I5", "I6", "I7", "I8", "I9", "tjena", "tjena2", "tjena3"], "warnings": []}
changed: [localhost -> localhost] => (item=gfal-ls) => {"changed": true, "cmd": ["gfal-ls", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO"], "delta": "0:00:01.869157", "end": "2017-02-03 14:42:47.270702", "item": "gfal-ls", "rc": 0, "start": "2017-02-03 14:42:45.401545", "stderr": "", "stdout": "A1\nC6\nA2\nF1\nA3\nA6\nC7\nBARZ6\nF2\ntjena\nA5\ntjena2\ntjena3\nA4\nF3\nA\nB\nF4\nC8\nBAR\nH6\nBARZ4\nG1\nE1\nG2\nH2\nE3\nI1\nI3\nH1\nH4\nI2\nI11\nC1\nI12\nH5\nI4\nI13\nI16\nC2\nC3\nD1\nH8\nC4\nD2\nC5\nI5\nH9\nD3\nD6\nD4\nH10\nI6\nD5\nH3\nD7\nI7\nI8\nI9\nI10\nI14\nI15", "stdout_lines": ["A1", "C6", "A2", "F1", "A3", "A6", "C7", "BARZ6", "F2", "tjena", "A5", "tjena2", "tjena3", "A4", "F3", "A", "B", "F4", "C8", "BAR", "H6", "BARZ4", "G1", "E1", "G2", "H2", "E3", "I1", "I3", "H1", "H4", "I2", "I11", "C1", "I12", "H5", "I4", "I13", "I16", "C2", "C3", "D1", "H8", "C4", "D2", "C5", "I5", "H9", "D3", "D6", "D4", "H10", "I6", "D5", "H3", "D7", "I7", "I8", "I9", "I10", "I14", "I15"], "warnings": []}

TASK [arcls SRM path / file] ***************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:97
changed: [localhost -> localhost] => (item=arcls) => {"changed": true, "cmd": ["arcls", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO/BARZ"], "delta": "0:00:01.534221", "end": "2017-02-03 14:42:49.067623", "failed": false, "failed_when_result": false, "item": "arcls", "rc": 1, "start": "2017-02-03 14:42:47.533402", "stderr": "ERROR: Failed to obtain information about file: No such file or directory: All ls requests failed in some way or another: No such file or directory /pnfs/csc.fi/data/cms/FOO/BARZ", "stdout": "", "stdout_lines": [], "warnings": []}
changed: [localhost -> localhost] => (item=gfal-ls) => {"changed": true, "cmd": ["gfal-ls", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO/BARZ"], "delta": "0:00:01.002051", "end": "2017-02-03 14:42:50.282821", "failed": false, "failed_when_result": false, "item": "gfal-ls", "rc": 2, "start": "2017-02-03 14:42:49.280770", "stderr": "gfal-ls error: 2 (No such file or directory) - Error reported from srm_ifce : 2 [SE][Ls][SRM_INVALID_PATH] No such file or directory /pnfs/csc.fi/data/cms/FOO/BARZ", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [arcls SRM path / file_xrdcp write] ***************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:104
changed: [localhost -> localhost] => {"changed": true, "cmd": ["arcls", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO/BARZ_write"], "delta": "0:00:01.594819", "end": "2017-02-03 14:42:52.124612", "failed": false, "failed_when_result": false, "rc": 1, "start": "2017-02-03 14:42:50.529793", "stderr": "ERROR: Failed to obtain information about file: No such file or directory: All ls requests failed in some way or another: No such file or directory /pnfs/csc.fi/data/cms/FOO/BARZ_write", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [arcls SRM path / file_webdav write] **************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:110
changed: [localhost -> localhost] => {"changed": true, "cmd": ["arcls", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO/BARZ_webdav_write"], "delta": "0:00:01.728513", "end": "2017-02-03 14:42:54.093477", "failed": false, "failed_when_result": false, "rc": 1, "start": "2017-02-03 14:42:52.364964", "stderr": "ERROR: Failed to obtain information about file: No such file or directory: All ls requests failed in some way or another: No such file or directory /pnfs/csc.fi/data/cms/FOO/BARZ_webdav_write", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [arcrm SRM file_xrdcp write first if it exists] ***************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:118
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [arcrm SRM file first if it exists] ***************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:124
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [arcrm SRM webdav_file first if it exists] ********************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:130
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [root:// endpoint listing] ************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:139
changed: [localhost -> localhost] => (item=xrdfs) => {"changed": true, "cmd": ["xrdfs", "root://se.example.com:1094/pnfs/csc.fi/data/cms/FOO"], "delta": "0:00:00.008643", "end": "2017-02-03 14:42:54.395657", "item": "xrdfs", "rc": 0, "start": "2017-02-03 14:42:54.387014", "stderr": "", "stdout": "\u001b[?1034h[se.example.com:1094] / > Goodbye.", "stdout_lines": ["\u001b[?1034h[se.example.com:1094] / > Goodbye."], "warnings": []}
changed: [localhost -> localhost] => (item=gfal-ls) => {"changed": true, "cmd": ["gfal-ls", "root://se.example.com:1094/pnfs/csc.fi/data/cms/FOO"], "delta": "0:00:00.816191", "end": "2017-02-03 14:42:55.380090", "item": "gfal-ls", "rc": 0, "start": "2017-02-03 14:42:54.563899", "stderr": "", "stdout": "A1\nC6\nA2\nF1\nA3\nA6\nC7\nBARZ6\nF2\ntjena\nA5\ntjena2\ntjena3\nA4\nF3\nA\nB\nF4\nC8\nBAR\nH6\nBARZ4\nG1\nE1\nG2\nH2\nE3\nI1\nI3\nH1\nH4\nI2\nI11\nC1\nI12\nH5\nI4\nI13\nI16\nC2\nC3\nD1\nH8\nC4\nD2\nC5\nI5\nH9\nD3\nD6\nD4\nH10\nI6\nD5\nH3\nD7\nI7\nI8\nI9\nI10\nI14\nI15", "stdout_lines": ["A1", "C6", "A2", "F1", "A3", "A6", "C7", "BARZ6", "F2", "tjena", "A5", "tjena2", "tjena3", "A4", "F3", "A", "B", "F4", "C8", "BAR", "H6", "BARZ4", "G1", "E1", "G2", "H2", "E3", "I1", "I3", "H1", "H4", "I2", "I11", "C1", "I12", "H5", "I4", "I13", "I16", "C2", "C3", "D1", "H8", "C4", "D2", "C5", "I5", "H9", "D3", "D6", "D4", "H10", "I6", "D5", "H3", "D7", "I7", "I8", "I9", "I10", "I14", "I15"], "warnings": []}

TASK [stat source_file] ********************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:147
ok: [localhost -> localhost] => {"changed": false, "stat": {"atime": 1486053901.5511982, "checksum": "5294292e00d5d607d2bd08409d029f6a0859b817", "ctime": 1481558750.8523169, "dev": 2050, "executable": true, "exists": true, "gid": 0, "gr_name": "root", "inode": 134321518, "isblk": false, "ischr": false, "isdir": false, "isfifo": false, "isgid": false, "islnk": false, "isreg": true, "issock": false, "isuid": false, "md5": "59189125c11a18c588b545bb17cd2e32", "mode": "0755", "mtime": 1481066373.0, "nlink": 1, "path": "/bin/bash", "pw_name": "root", "readable": true, "rgrp": true, "roth": true, "rusr": true, "size": 960472, "uid": 0, "wgrp": false, "woth": false, "writeable": false, "wusr": true, "xgrp": true, "xoth": true, "xusr": true}}

TASK [store source_file checksum in a fact] ************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:151
ok: [localhost -> localhost] => {"ansible_facts": {"source_checksum": "5294292e00d5d607d2bd08409d029f6a0859b817"}, "changed": false}

TASK [arccp SRM here to there] *************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:157
NOTIFIED HANDLER cleanup
changed: [localhost -> localhost] => {"changed": true, "cmd": ["arccp", "/bin/bash", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO/BARZ"], "delta": "0:02:56.510790", "end": "2017-02-03 14:45:52.482241", "rc": 0, "start": "2017-02-03 14:42:55.971451", "stderr": "", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [arccp SRM there to here] *************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:162
NOTIFIED HANDLER cleanup2
changed: [localhost -> localhost] => {"changed": true, "cmd": ["arccp", "srm://se.example.com:8443/pnfs/csc.fi/data/cms/FOO/BARZ", "/tmp/BARZ"], "delta": "0:00:17.784664", "end": "2017-02-03 14:46:10.523137", "rc": 0, "start": "2017-02-03 14:45:52.738473", "stderr": "", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [xrdcp there to here] *****************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:167
NOTIFIED HANDLER cleanup3
changed: [localhost -> localhost] => {"changed": true, "cmd": ["xrdcp", "root://se.example.com:1094/pnfs/csc.fi/data/cms/FOO/BARZ", "/tmp/BARZ2"], "delta": "0:00:08.193078", "end": "2017-02-03 14:46:18.966717", "rc": 0, "start": "2017-02-03 14:46:10.773639", "stderr": "\r[938kB/938kB][100%][==================================================][117.2kB/s]  \r[938kB/938kB][100%][==================================================][117.2kB/s]  ", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [xrdcp here to there - fail if return code is not xrdcp_write_expected_return_code] ***
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:174
NOTIFIED HANDLER cleanup_xrdcp_write
changed: [localhost -> localhost] => {"changed": true, "cmd": ["xrdcp", "/bin/bash", "root://se.example.com:1094/pnfs/csc.fi/data/cms/FOO/BARZ_write"], "delta": "0:00:24.801765", "end": "2017-02-03 14:46:44.017826", "failed": false, "failed_when_result": false, "rc": 0, "start": "2017-02-03 14:46:19.216061", "stderr": "\r[938kB/938kB][100%][==================================================][156.3kB/s]  \r[938kB/938kB][100%][==================================================][39.08kB/s]  ", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [arccp webdav here to there] **********************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:183
NOTIFIED HANDLER cleanup_webdav_write
changed: [localhost -> localhost] => {"changed": true, "cmd": ["arccp", "/bin/bash", "http://se.example.com:2880:2880/pnfs/csc.fi/data/cms/FOO/BARZ_write_webdav"], "delta": "0:00:00.409518", "end": "2017-02-03 14:46:44.677504", "failed": false, "failed_when_result": false, "rc": 1, "start": "2017-02-03 14:46:44.267986", "stderr": "ERROR: Current transfer FAILED: Failed while writing to destination: Permission denied: Unauthorized", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [arccp webdav there to here] **********************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:190
changed: [localhost -> localhost] => {"changed": true, "cmd": ["arccp", "http://se.example.com:2880:2880/pnfs/csc.fi/data/cms/FOO/BARZ", "/tmp/BARZ6"], "delta": "0:00:09.344078", "end": "2017-02-03 14:46:54.269283", "rc": 0, "start": "2017-02-03 14:46:44.925205", "stderr": "", "stdout": "", "stdout_lines": [], "warnings": []}

TASK [stat fetched source_file] ************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:197
ok: [localhost -> localhost] => {"changed": false, "stat": {"atime": 1486125953.4744174, "checksum": "5294292e00d5d607d2bd08409d029f6a0859b817", "ctime": 1486125969.3633087, "dev": 2050, "executable": false, "exists": true, "gid": 1000, "gr_name": "admin", "inode": 203137403, "isblk": false, "ischr": false, "isdir": false, "isfifo": false, "isgid": false, "islnk": false, "isreg": true, "issock": false, "isuid": false, "md5": "59189125c11a18c588b545bb17cd2e32", "mode": "0600", "mtime": 1486125969.3633087, "nlink": 1, "path": "/tmp/BARZ", "pw_name": "myusername", "readable": true, "rgrp": false, "roth": false, "rusr": true, "size": 960472, "uid": 5003, "wgrp": false, "woth": false, "writeable": true, "wusr": true, "xgrp": false, "xoth": false, "xusr": false}}

TASK [stat fetched source_file2] ***********************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:202
ok: [localhost -> localhost] => {"changed": false, "stat": {"atime": 1486125973.6722794, "checksum": "5294292e00d5d607d2bd08409d029f6a0859b817", "ctime": 1486125978.9242435, "dev": 2050, "executable": false, "exists": true, "gid": 1000, "gr_name": "admin", "inode": 203334441, "isblk": false, "ischr": false, "isdir": false, "isfifo": false, "isgid": false, "islnk": false, "isreg": true, "issock": false, "isuid": false, "md5": "59189125c11a18c588b545bb17cd2e32", "mode": "0644", "mtime": 1486125978.9242435, "nlink": 1, "path": "/tmp/BARZ2", "pw_name": "myusername", "readable": true, "rgrp": true, "roth": true, "rusr": true, "size": 960472, "uid": 5003, "wgrp": false, "woth": false, "writeable": true, "wusr": true, "xgrp": false, "xoth": false, "xusr": false}}

TASK [debug print checksums of fetched files] **********************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:207
ok: [localhost -> localhost] => (item=reg_source_1) => {
    "item": "reg_source_1", 
    "reg_source_1.stat.checksum": "5294292e00d5d607d2bd08409d029f6a0859b817"
}
ok: [localhost -> localhost] => (item=reg_source_2) => {
    "item": "reg_source_2", 
    "reg_source_2.stat.checksum": "5294292e00d5d607d2bd08409d029f6a0859b817"
}

TASK [assert that checksums are all the same] **********************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:214
ok: [localhost -> localhost] => (item=reg_source_1) => {
    "changed": false, 
    "item": "reg_source_1", 
    "msg": "All assertions passed"
}
ok: [localhost -> localhost] => (item=reg_source_2) => {
    "changed": false, 
    "item": "reg_source_2", 
    "msg": "All assertions passed"
}

TASK [xrdcp from xrootd federation server1 to here] ****************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:225
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [xrdcp from xrootd federation server2 to here] ****************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:230
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [xrdcp from xrootd federation server3 to here] ****************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:235
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [ldapsearch tool comes from openldap-clients on redhat] *******************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:242
changed: [localhost -> localhost] => {"changed": true, "cmd": ["rpm", "-qa", "openldap-clients"], "delta": "0:00:00.714090", "end": "2017-02-03 14:46:55.902678", "failed": false, "failed_when_result": false, "rc": 0, "start": "2017-02-03 14:46:55.188588", "stderr": "", "stdout": "openldap-clients-2.4.40-13.el7.x86_64", "stdout_lines": ["openldap-clients-2.4.40-13.el7.x86_64"], "warnings": ["Consider using yum, dnf or zypper module rather than running rpm"]}
 [WARNING]: Consider using yum, dnf or zypper module rather than running rpm


TASK [ldapsearch tool comes from ldap-utils on debian] *************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:248
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

TASK [shell ldapsearch towards ldap_server port ldap_port and grep for ldap_gluename] ***
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:254
ok: [localhost -> localhost] => {"changed": false, "cmd": "ldapsearch -LL -x -H ldap://se.example.com:2170 -b o=grid GlueSESizeTotal|grep -c ^GlueSESizeTotal", "delta": "0:00:00.115580", "end": "2017-02-03 14:46:56.282749", "failed": false, "failed_when_result": false, "rc": 0, "start": "2017-02-03 14:46:56.167169", "stderr": "", "stdout": "1", "stdout_lines": ["1"], "warnings": []}

TASK [debug print ldapsearch] **************************************************
task path: /home/myusername/git/ansible-dcache_functionality_test/se_test.yml:261
ok: [localhost -> localhost] => {
    "reg_ldapsearch": {
        "changed": false, 
        "cmd": "ldapsearch -LL -x -H ldap://se.example.com:2170 -b o=grid GlueSESizeTotal|grep -c ^GlueSESizeTotal", 
        "delta": "0:00:00.115580", 
        "end": "2017-02-03 14:46:56.282749", 
        "failed": false, 
        "failed_when_result": false, 
        "rc": 0, 
        "start": "2017-02-03 14:46:56.167169", 
        "stderr": "", 
        "stdout": "1", 
        "stdout_lines": [
            "1"
        ], 
        "warnings": []
    }
}

RUNNING HANDLER [cleanup] ******************************************************
changed: [localhost] => {"changed": true, "cmd": ["arcrm", "srm://se.example.com/pnfs/csc.fi/data/cms/FOO/BARZ"], "delta": "0:00:01.877685", "end": "2017-02-03 14:46:58.392726", "rc": 0, "start": "2017-02-03 14:46:56.515041", "stderr": "", "stdout": "", "stdout_lines": [], "warnings": []}

RUNNING HANDLER [cleanup2] *****************************************************
changed: [localhost] => {"changed": true, "path": "/tmp/BARZ", "state": "absent"}

RUNNING HANDLER [cleanup3] *****************************************************
changed: [localhost] => (item=2) => {"changed": true, "item": 2, "path": "/tmp/BARZ2", "state": "absent"}
ok: [localhost] => (item=3) => {"changed": false, "item": 3, "path": "/tmp/BARZ3", "state": "absent"}
ok: [localhost] => (item=4) => {"changed": false, "item": 4, "path": "/tmp/BARZ4", "state": "absent"}
ok: [localhost] => (item=5) => {"changed": false, "item": 5, "path": "/tmp/BARZ5", "state": "absent"}
changed: [localhost] => (item=6) => {"changed": true, "item": 6, "path": "/tmp/BARZ6", "state": "absent"}

RUNNING HANDLER [cleanup_xrdcp_write] ******************************************
changed: [localhost] => {"changed": true, "cmd": ["arcrm", "srm://se.example.com/pnfs/csc.fi/data/cms/FOO/BARZ_write"], "delta": "0:00:00.608786", "end": "2017-02-03 14:47:00.635925", "rc": 0, "start": "2017-02-03 14:47:00.027139", "stderr": "", "stdout": "", "stdout_lines": [], "warnings": []}

RUNNING HANDLER [cleanup_webdav_write] *****************************************
skipping: [localhost] => {"changed": false, "skip_reason": "Conditional check failed", "skipped": true}

PLAY RECAP *********************************************************************
localhost                  : ok=28   changed=16   unreachable=0    failed=0   


</pre>

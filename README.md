# Mattermost for OpenShift 3

This is instant mattermost application for OpenShift 3.

The license applies to all files insinde this repository, not mattermost itself.

## Prerequisites

OpenShift 3 up and running, including the capability to create a new project.

## Installation

```shell
$ oc new-project mattermost

# CENTOS-based (works in OpenShift Online)
$ oc new-app https://raw.githubusercontent.com/RHsyseng/mattermost-openshift/master/mattermost-centos.yaml

# RHEL-based
$ oc new-app https://raw.githubusercontent.com/RHsyseng/mattermost-openshift/master/mattermost.yaml

# OR for new dedicated env(s) in same project space 

$ oc new-app https://raw.githubusercontent.com/RHsyseng/mattermost-openshift/master/mattermost.yaml \
-p APPLICATION_NAME=acme
```

####For persistent deployments
You need to provision a PV:
```
$ cat mattermost-pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteOnce
  nfs:
    path: /srv/nfs/path
    server: nfs-server
  persistentVolumeReclaimPolicy: Retain

$ oc create -f mattermost-pv.yaml

$ oc new-app -f https://raw.githubusercontent.com/RHsyseng/mattermost-openshift/master/db-persistent.yaml \
-f https://raw.githubusercontent.com/RHsyseng/mattermost-openshift/master/mattermost.yaml -p MYSQL_PASSWORD=mmtest
```

## Usage

Point your browser at your OCP route/url, the first user you create will
be an Administrator of mattermost.

## Copyright

Copyright (C) 2017 Red Hat Inc.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.

The GNU General Public License is provided within the file LICENSE.
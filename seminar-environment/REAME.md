##### SETUP JENKINS #####

1) Need to create persistence volume

>>>> jenkins-volume.json
>>>> Run as 'root' under default project
{
  "apiVersion": "v1",
  "kind": "PersistentVolume",
  "metadata": {
    "name": "jenkins-volume"
  },
  "spec": {
    "capacity": {
        "storage": "256Mi"
        },
    "accessModes": [ "ReadWriteMany" ],
    "nfs": {
        "path": "/app/data/jenkins",
        "server": "rhel7ose1.mycloud"
    }
  }
}

>>>> jenkins-claim.json
>>>> Run under project 'jenkins'
{
  "apiVersion": "v1",
  "kind": "PersistentVolumeClaim",
  "metadata": {
    "name": "jenkins-claim"
  },
  "spec": {
    "accessModes": [ "ReadWriteMany" ],
    "resources": {
      "requests": {
        "storage": "256Mi"
      }
    }
  }
}

2) Verify the volumns
$ oc get pv
NAME             LABELS    CAPACITY    ACCESSMODES   STATUS    CLAIM                   REASON
jenkins-volume   <none>    268435456   RWX           Bound     jenkins/jenkins-claim   

$ oc get pvc
NAME            LABELS    STATUS    VOLUME
jenkins-claim   map[]     Bound     jenkins-volume

3) Create the jenkins template
$ oc project jenkins
$ oc create -f jenkins.json




##### SETUP GOGS #####

1) Need to create persistence volume

>>>> gogs-volume.json
>>>> Run as 'root' under default project
{
  "apiVersion": "v1",
  "kind": "PersistentVolume",
  "metadata": {
    "name": "gogs-volume"
  },
  "spec": {
    "capacity": {
        "storage": "512Mi"
        },
    "accessModes": [ "ReadWriteMany" ],
    "nfs": {
        "path": "/app/data/gogs",
        "server": "rhel7ose1.mycloud"
    }
  }
}

>>>> gogs-claim.json
>>>> Run under project 'gogs'
{
  "apiVersion": "v1",
  "kind": "PersistentVolumeClaim",
  "metadata": {
    "name": "gogs-claim"
  },
  "spec": {
    "accessModes": [ "ReadWriteMany" ],
    "resources": {
      "requests": {
        "storage": "512Mi"
      }
    }
  }
}

2) Verify the volumns
$ oc get pv
NAME             LABELS    CAPACITY    ACCESSMODES   STATUS    CLAIM                   REASON
gogs-volume      <none>    536870912   RWX           Bound     gogs/gogs-claim         
jenkins-volume   <none>    268435456   RWX           Bound     jenkins/jenkins-claim   

$ oc get pvc
NAME         LABELS    STATUS    VOLUME
gogs-claim   map[]     Bound     gogs-volume


3) Create the gogs template
$ oc project gogs
$ oc create -f gogs.json


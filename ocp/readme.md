# OCP usage
refer: https://pittar.medium.com/running-oracle-12c-on-openshift-container-platform-ca471a9f7057

*  A new Project (namespace in Kubernetes terms) called “informix2”
```shell
$ oc login -u system:admin
$ oc project informix2
```
*  A service account called “informix-sa” with elevated privileges to run Informix.
```shell
oc create sa informix-sa
```
* We apply the seetings in ocp

```shell
oc import-image informix-innovator-c:14.10.FC7W1IE --from="ibmcom/informix-innovator-c:14.10.FC7W1IE" --confirm --reference-policy=local
oc process -f .\informix-template.yaml  | oc create  -f  -
oc process -f .\informix-template.yaml  | oc delete  -f  -
```
* login as pod

```shell
PODMAN=$( oc   get pods  --no-headers -o custom-columns=":metadata.name" --field-selector status.phase=Running |grep informix)
echo $PODMAN
winpty kubectl  exec -it  $PODMAN  --  bash

[informix@informix-1-jrwfz ~]$ dbaccess
```

**⚠ TIP:**   You need to cinfgiure the informix database use **logging mode** in  dbacess:
```shell
SQL:   New  Run  Modify  Use-editor  Output  Choose  Save  Info  Drop  Exit
Run the current SQL statements.

----------------------- commondb@informix ------ Press CTRL-W for Help --------

create database commondb with log;
```

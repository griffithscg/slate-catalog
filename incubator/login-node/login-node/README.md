# Login node 
This chart contains an installation of an SSH login to a CentOS container, provisioned by Kubernetes. 

# Installation
To install the helm chart on your Kubernetes cluster, move outside this directory and use the command:
$ helm install ./login-node

You should see something like this - 

NAME:   dining-ragdoll
LAST DEPLOYED: Mon Jul 22 14:15:22 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                       DATA  AGE
dining-ragdoll-login-node  1     1s

==> v1/Deployment
NAME                       READY  UP-TO-DATE  AVAILABLE  AGE
dining-ragdoll-login-node  0/1    1           0          1s

==> v1/Pod(related)
NAME                                        READY  STATUS             RESTARTS  AGE
dining-ragdoll-login-node-686bfd4c87-zc99f  0/1    ContainerCreating  0         1s

==> v1/Service
NAME                               TYPE      CLUSTER-IP      EXTERNAL-IP  PORT(S)       AGE
dining-ragdoll-login-node-service  NodePort  10.109.171.119  <none>       22:32125/TCP  1s

Check that the deployment has been been created, and the pod is running by using the commands $kubectl describe deployment [deployment_name],
and $kubectl describe pod [pod_name]. 

# Adding users 
To add users, modify the values.yaml file. Currently there are dummy values in the config file data, make sure to replace these before ssh'ing. 
The format for adding a user is [user_name]:[password]:[user_id]:[group_id]::/home/[user_name]:/bin/bash:[user_public_key]. Make sure to leave an empty line 
after each user you add. 

# SSH
To find the IP address of the node that the pod is running on, use kubectl describe nodes, and look for the pod.
The IP address of the node should be in InternalIP under addresses, right above where the pods are listed. To 
find the port use kubectl describe service [service_name]. Then, you can ssh in using 
$ ssh -i [user_private_key] -p [port_no] [user_name]@[IP]

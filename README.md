# tekton-workspaces - Openshift Cluster

# 1. Install pipelines operator
<img width="960" alt="image" src="https://github.com/Gokul-C/tekton-workspaces/assets/46756853/cb2d29c3-273e-433b-99df-81a72874d6bb">

After installing openshift pipelines operator, it will create one service account called "pipeline" in cluster. This service account is responsible for creating components in cluster. So we need to give permissions to this service account.

# 2. Give cluster-admin permission to the SA
```
oc adm policy add-cluster-role-to-user cluster-admin -z pipeline 
```
# 3. Check default storage class
```
kubectl get sc
```
It will be automatically created while creating the cluster. If in your storage class if you see word local then your storage class is local-storage

# 4. set label for any worker node
```
kubectl label nodes <your-node-name> disktype=ssd
```
This is used to create node-affinity while creating persistent volumes. We must add node-affinity while creating local-storage persistant volumes

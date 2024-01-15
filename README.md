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

# 5. Create pv-pvc 
```
kubectl create -f https://raw.githubusercontent.com/Gokul-C/tekton-workspaces/main/pv-pvc.yaml
```
By applying this file pv and pvc will be created and pvc is in "waiting for first consumer" state

# 6. Create tasks
```
https://raw.githubusercontent.com/Gokul-C/tekton-workspaces/main/tasks.yaml
```
# 7. Create pipeline
```
https://raw.githubusercontent.com/Gokul-C/tekton-workspaces/main/pipeline.yaml
```
# 8. Create Pipelinerun
```
https://github.com/Gokul-C/tekton-workspaces/blob/main/pipelinerun.yaml
```
# Summary:
Here we are creating two tasks one will write text into a file and another one will read it so these two tasks will run on two different containers so to pass the information between tasks we need persistent volume. So we are creating persistant volume and persistent volume claim. Then We have created tasks, pipeline & pipelinerun. In pipelinerun we have mentioned the persistant volume claim name. Now if you type ```kubectl get pods```
You see the pods that are created automatically by pipelinerun and successfully running.
```
kubectl get pods
```
<img width="422" alt="image" src="https://github.com/Gokul-C/tekton-workspaces/assets/46756853/7e8a35ba-ab34-4ccc-8754-7fc27ce439aa">

```
kubectl logs pipeline-run-initial-task-pod
```
<img width="521" alt="image" src="https://github.com/Gokul-C/tekton-workspaces/assets/46756853/36b30d6f-07bc-4f1a-8319-53331a181992">

```
kubectl logs pipeline-run-final-task-pod
```
<img width="507" alt="image" src="https://github.com/Gokul-C/tekton-workspaces/assets/46756853/3ba7a8c2-7ae6-4d86-8723-210059059edf">



## Problem Statement

Tanzu Application Platform leverages Cartographer's Supply chain to orchestrate the CI/CD Process. However TAP works with tools installed within the TAP Cluster like Tekton, Flux, Grype etc. 
In this Gist, we will look at integrating TAP with external tools like ServiceNow.

### Note

For the purpose of this Demo - We took the liberty to integrate TAP Supply chain with the Snow's **Incident** module. However integrating with other modules like ChangeRequest, RITM, Task follows the same pattern. The only change would be to update the API call to point to the other modules.

## Does this work for other integrations ?

Yes, this integration is not limited to just ServiceNow - the tekton scripts attached to gist will work with any rest api. All thats required is to update the respective API before kicking off the supply chain.

## How does External Integrations work with Tanzu Application Platform ?

Tanzu Application Plaform uses Tekton to get any custom integrations accomplished. As of this gist, there is no other plugin that integrates external tools.

In order to create a custom integration - below is the order in which different templates need to be created

```
# supplychain 
#     clustersourcetemplate
#           runnable
#                 ref: clusterruntemplate
#                           pipelinerun
#                                  pipeline
```

* A custom supply chain that references the clustersourcetemplate in the steps.

* A clustersourcetemplate that references the runnable and clusterruntemplate specific to the SNOW Integreation 

* A Tekton PipelineRun that references the selector labels of the pipeline.

* A pipeline that does the actually integration itself. 


The above scripts are available in the github repo linked [here](https://github.com/gowthamshankar99/tanzu_tap_snow_integration). 

## Test the Integration 

 * Submit the workload pointing to the label selector of the new custom supply chain referencing the snow integration. Link on how to submit your first workload is [here](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-getting-started-deploy-first-app.html).
 
 * Access the tap-gui to see if the step is waiting for the snow approval 
 
 * Navigate to the snow portal and approve/resolve the ticket.
 
 * Verify if the supply chain picked the changes up and deployed to the code to the respective environment upon approval/resolution.





---
Description: 'Creates a new job on the HPC cluster, for which the specified properties have the specified values.'
audience: developer
author: 'REDMOND\\danlep'
manager: 'REDMOND\\timlt'
ms.assetid: 'D6585D16-4040-461D-BBC6-788E5B4C458F'
ms.prod: 'windows-server-dev'
ms.technology: 'compute-cluster-pack'
ms.tgt_platform: multiple
title: Create Job
---

# Create Job

Creates a new job on the HPC cluster, for which the specified properties have the specified values.

## Request

You can specify the Create Job request as follows.



| Method                      | Request URI                                                            |
|-----------------------------|------------------------------------------------------------------------|
| POST (before HPC Pack 2016) | https://*head\_node\_name*:*port*/WindowsHPC/*HPC\_cluster\_name*/Jobs |
| POST (HPC Pack 2016)        | https://*head\_node\_name*:*port*/WindowsHPC/Jobs                      |



�

For instances of the REST web service that are hosted in Azure, the head node name should have a format of *Windows\_Azure\_service\_name*.cloudapp.net.

To get the name to use for an HPC cluster that runs at least Microsoft HPC Pack 2008 R2 with Service Pack 3 (SP3), use the [**Get Clusters**](get-clusters.md) operation.

### URI Parameters

You can specify the following additional parameters on the request URI.



| Parameter   | Description                                                                                                                                                                                                                                                                                                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| api-version | Optional. Specifies the version of the operation to use for this request. To specify Microsoft�HPC�Pack�2008�R2 with Service�Pack�3 (SP3), use a value of 2011-11-01. The minimum version that supports this URI parameter is Microsoft�HPC�Pack�2008�R2 with SP3.<br/> The value of this URI parameter is ignored if the request also contains the api-version header.<br/> |



�

### Request Headers

The following table describes required and optional request headers.



| Request Header | Description                                                                                                                                                                                                                                                                                                                                                                               |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| api-version    | Optional. Specifies the version of the operation to use for this request. To specify Microsoft�HPC�Pack�2008�R2 with SP3, use a value of 2011-11-01. The minimum version that supports this header is Microsoft�HPC�Pack�2008�R2 with SP3.<br/> The value specified in this header overrides the value specified in the api-version URI parameter if both are specified.<br/> |
| CCP\_Version   | Optional. Specifies the version of the operation to use for this request.<br/> Deprecated beginning with Microsoft HPC Pack 2008 R2 with Service Pack 3 (SP3).<br/>                                                                                                                                                                                                           |



�

### Request Body

The format of the request body is as follows.


```XML
<ArrayOfProperty xmlns="http://schemas.microsoft.com/HPCS2008R2/common" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <Property>
        <Name>Property Name</Name> 
        <Value>Property Value</Value>
    </Property>
    <Property>
        <Name>Property Name</Name>
        <Value>Property Value</Value>
    </Property>
    �
</ArrayOfProperty>
```



The following table describes each of the elements in this request XML.



| Element         | Description                                                                                                                                                                                             |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ArrayOfProperty | Represents the set of properties for the job for which you want to set values. To create a job using the default values for all of the properties, use an empty **ArrayOfProperty** element.<br/> |
| Property        | Represents a single property for the job.<br/>                                                                                                                                                    |
| Name            | Contains the name of the property that you want to set.<br/>                                                                                                                                      |
| Value           | Contains the value to which you want to set the property.<br/>                                                                                                                                    |



�

## Response

The response includes an HTTP status code, a set of response headers, and a response body in XML format.

### Status Code

A successful operation returns status code 200 (OK). For more information about status codes, see [HttpStatusCode](https://msdn.microsoft.com/library/system.net.httpstatuscode.aspx).

The error response is dependent upon the api-version used in the request. If the api-version is not provided, or CCP\_Version is specified, then the error response will be an XML string (Note: The error message will vary based on the error):


```XML
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Error message text.</string>
```



If the api-version is 2011-11-01 or later, the error message will be a more descriptive XML response (Note: Values will vary based on the error):


```XML
<HpcWebServiceFault xmlns="http://schemas.microsoft.com/HPCS2008R2/common" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <Code>267386880</Code>
  <Message>Error message text.</Message>
  <Values i:nil="true" xmlns:a="http://schemas.datacontract.org/2004/07/System.Collections.Generic"/>
</HpcWebServiceFault>
```



### Response Headers

The response for this operation includes the following headers.



| Response Header         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| x-ms-hpc-authoritychain | A comma-separated list of RFC 1918 IP addresses that indicate the sequence of instances of the REST web service that the operation called in order from to right.<br/> Only responses from instances of the REST web service that are hosted on Azure contain this header. Responses from instances of the REST web service that are hosted on premise omit this header.<br/> This header is supported beginning with Microsoft HPC Pack 2008 R2 with SP3 and is not available in previous versions.<br/> |



�

The response for this operation also includes standard HTTP headers. All standard headers conform to the [HTTP/1.1 protocol specification](http://www.w3.org/Protocols/rfc2616/rfc2616.mdl).

### Response Body


```XML
<int xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Job Identifier</int>
```



The response XML consists of a single int element that contains the job identifier of the job that the HPC Job Scheduler Service created.

## Remarks

The job that this operation creates has no tasks. You must add a task to the job by using the Add Task operation before you submit the job to the HPC cluster with the Submit Job operation.

The following table shows the job properties for which you can set the values. For information about the property, see the corresponding property of the ISchedulerJob interface or JobPropertyIds class in the managed API for Microsoft HPC Pack. To set the value of properties that have an [IStringCollection](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.istringcollection.aspx) interface for a value in the managed API, such as RequestedNodes, use a comma-separated list of values in the Value element of the request.



| Property               | Corresponding ISchedulerJob or JobPropertyIds Property                                                                                       |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| Name                   | [ISchedulerJob.Name](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.name.aspx)                                     |
| UserName               | [ISchedulerJob.UserName](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.username.aspx)                             |
| Project                | [ISchedulerJob.Project](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.project.aspx)                               |
| RuntimeSeconds         | [ISchedulerJob.Runtime](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.runtime.aspx)                               |
| MinCores               | [ISchedulerJob.MinimumNumberOfCores](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.minimumnumberofcores.aspx)     |
| MaxCores               | [ISchedulerJob.MaximumNumberOfCores](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.maximumnumberofcores.aspx)     |
| MinNodes               | [ISchedulerJob.MinimumNumberOfNodes](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.minimumnumberofnodes.aspx)     |
| MaxNodes               | [ISchedulerJob.MaximumNumberOfNodes](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.maximumnumberofnodes.aspx)     |
| MinSockets             | [ISchedulerJob.MinimumNumberOfSockets](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.minimumnumberofsockets.aspx) |
| MaxSockets             | [ISchedulerJob.MaximumNumberOfSockets](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.maximumnumberofsockets.aspx) |
| UnitType               | [ISchedulerJob.UnitType](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.unittype.aspx)                             |
| RequestedNodes         | [ISchedulerJob.RequestedNodes](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.requestednodes.aspx)                 |
| IsExclusive            | [ISchedulerJob.IsExclusive](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.isexclusive.aspx)                       |
| RunUntilCanceled       | [ISchedulerJob.RunUntilCanceled](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.rununtilcanceled.aspx)             |
| NodeGroups             | [ISchedulerJob.NodeGroups](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.nodegroups.aspx)                         |
| FailOnTaskFailure      | [ISchedulerJob.FailOnTaskFailure](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.failontaskfailure.aspx)           |
| AutoCalculateMax       | [ISchedulerJob.AutoCalculateMax](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.autocalculatemax.aspx)             |
| AutoCalculateMin       | [ISchedulerJob.AutoCalculateMin](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.autocalculatemin.aspx)             |
| Preemptable            | [ISchedulerJob.CanPreempt](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.canpreempt.aspx)                         |
| MinMemory              | [ISchedulerJob.MinMemory](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.minmemory.aspx)                           |
| MaxMemory              | [ISchedulerJob.MaxMemory](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.maxmemory.aspx)                           |
| MinCoresPerNode        | [ISchedulerJob.MinCoresPerNode](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.mincorespernode.aspx)               |
| MaxCoresPerNode        | I[SchedulerJob.MaxCoresPerNode](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.maxcorespernode.aspx)               |
| SoftwareLicense        | [ISchedulerJob.SoftwareLicense](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.softwarelicense.aspx)               |
| OrderBy                | [ISchedulerJob.OrderBy](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.orderby.aspx)                               |
| ClientSource           | [ISchedulerJob.ClientSource](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.clientsource.aspx)                     |
| Progress               | [ISchedulerJob.Progress](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.progress.aspx)                             |
| ProgressMessage        | [ISchedulerJob.ProgressMessage](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.progressmessage.aspx)               |
| TargetResourceCount    | [ISchedulerJob.TargetResourceCount](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.targetresourcecount.aspx)       |
| ExpandedPriority       | [ISchedulerJob.ExpandedPriority](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.expandedpriority.aspx)             |
| ServiceName            | [ISchedulerJob.ServiceName](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.servicename.aspx)                       |
| NotifyOnStart          | [ISchedulerJob.NotifyOnStart](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.notifyonstart.aspx)                   |
| NotifyOnCompletion     | [ISchedulerJob.NotifyOnCompletion](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.notifyoncompletion.aspx)         |
| EmailAddress           | [ISchedulerJob.EmailAddress](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.emailaddress.aspx)                     |
| Priority               | [ISchedulerJob.Priority](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.priority.aspx)                             |
| Password               | [JobPropertyIds.Password](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.properties.jobpropertyids.password.aspx)                |
| JobTemplate            | [ISchedulerJob.JobTemplate](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.jobtemplate.aspx)                       |
| Pool                   | [ISchedulerJob.Pool](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.pool.aspx)                                     |
| JobValidExitCodes      | [ISchedulerJob.ValidExitCodes](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.validexitcodes.aspx)                 |
| ParentJobIds           | [ISchedulerJob.ParentJobIds](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.parentjobids.aspx)                     |
| FailDependentTasks     | [ISchedulerJob.FailDependentTasks](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.faildependenttasks.aspx)         |
| NodeGroupOp            | [ISchedulerJob.NodeGroupOp](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.nodegroupop.aspx)                       |
| SingleNode             | [ISchedulerJob.SingleNode](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.singlenode.aspx)                         |
| ChildJobIds            | [ISchedulerJob.ChildJobIds](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.childjobids.aspx)                       |
| EstimatedProcessMemory | [ISchedulerJob.EstimatedProcessMemory](https://msdn.microsoft.com/library/microsoft.hpc.scheduler.ischedulerjob.estimatedprocessmemory.aspx) |



�

## Requirements



|                    |                                                                                |
|--------------------|--------------------------------------------------------------------------------|
| Product<br/> | HPC Pack 2008 R2 with at least SP2, or a later version of HPC Pack.<br/> |



�

�




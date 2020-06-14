---
layout: default
title:  "Amazon ECS quick intro"
description: Article contains a brief introduction about the entities involved in Amazon ecs. This also includes a brief comparison between kubernetes and amazon ecs, difference between initContainers and Container ordering.
date:   2020-06-14 00:00:00 -0000
categories: main
---
# Amazon ECS quick intro:

This article aims to brief about the different entities involved in amazon ecs.

#### Entities in Amazon ECS:

1. **Task**:
  This is a wrapper around atomic entity in ecs realm, which is containers. A task can contain multiple containers. 
  Tasks are created from task definitions which we will look in following sections.

2. **Container**:
  This is nothing but a docker/ container image that runs the application.

3. **Container definitions:** 
  Container definitions as the name suggest contains the configuration required to spawn the container instances. A container definition will be declared inside task definition (Which we will see next). Few of those configurations includes:
  
    - *Container url*:
      Url from which to pull the docker image from

    - *Port mappings*:
      List of host to container port mapping 

    - *Container entrypoint*:
      Command to run the container after pulling the docker image

    - *Cloudwatch log stream*:
      Grouping the logs form tje container to specific clodwatch log streams.

    - *Environment variables*: 
      Environment variable specific to the container

    - *Container ordering*: 
      This requires special mention, as it is unique to amazon ecs. This is something similar 
      to _**initContainer** in Kubernetes, but not the same as it_. We can mention order of container execution within the tasks.
      Unlike initContainer in Kubernetes, which will be run for several instances of the container only once, The first container mentioned in the container ordering will run X times if the X number of container instances are spawned. Thus initContainer is per Service(entity wraping container in Kubernetes) where as Container ordering is per container.

      
4. **Task definition:**
  From programmer's perspective, it's like a class and Task are like an object.
  There will be multiple versions of Task definition. There can be multiple container definitions inside task defintion. In this we can give various configurations, few of which:
  - Task execution IAM role
  - Task size
  - Network mode: The networking
  - Container definitions: Already described above. A task can have multiple containers, this can be achieved by declaring multiple container definitions within a single task definition.  
  
    **Note**: Task definition doesn't have anything like task ordering to achieve something like initContainer

5. **Services:**
  A service is a wrapper around a task. Service is created from specific version of a single task definition. 
  If we want to spawn multiple container instances, service creation is the place where we can achieve this. To 
  summarize, A service contains multiple instances of task created from the same task definition and same task 
  definition version. 
  
    Here we can specify few other things like deployment startegy, forcing new deployment, which cluster the 
    service should attach itself to. 
  
    **Note**: If we want to redeploy our application, we can create a service with desired number of tasks and 
    select the same task definition and version and check the option of **force new deployment**

6. **Cluster:**
  At a high level cluster is a wrapper around services. [read more](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/clusters.html)
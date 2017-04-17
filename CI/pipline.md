## What is Pipeline? {#overview}

Jenkins Pipeline is a suite of plugins which supports implementing and integrating continuous delivery pipelines into Jenkins. Pipeline provides an extensible set of tools for modeling simple-to-complex delivery pipelines "as code" via the[Pipeline DSL](https://jenkins.io/doc/book/pipeline/syntax/).\[[1](https://jenkins.io/doc/book/pipeline/#_footnote_1)\]

Typically, this "Pipeline as Code" would be written to a[`Jenkinsfile`](https://jenkins.io/doc/book/pipeline/jenkinsfile/)and checked into a project’s source control repository, for example:

Jenkinsfile \(Declarative Pipeline\)

```
pipeline {
    agent any 


    stages {
        stage(
'
Build
'
) { 

            steps { 

                sh 
'
make
'

            }
        }
        stage(
'
Test
'
){
            steps {
                sh 
'
make check
'

                junit 
'
reports/**/*.xml
'

            }
        }
        stage(
'
Deploy
'
) {
            steps {
                sh 
'
make publish
'

            }
        }
    }
}
```

[Toggle Scripted Pipeline](https://jenkins.io/doc/book/pipeline/#)

_\(Advanced\)_

|  | [`agent`](https://jenkins.io/doc/book/pipeline/#agent)indicates that Jenkins should allocate an executor and workspace for this part of the Pipeline. |
| :--- | :--- |
|  | [`stage`](https://jenkins.io/doc/book/pipeline/#stage)describes a stage of this Pipeline. |
|  | [`steps`](https://jenkins.io/doc/book/pipeline/#step)describes the steps to be run in this`stage` |
|  | `sh`executes the given shell command |
|  | `junit`is a Pipeline[step](https://jenkins.io/doc/book/pipeline/#step)provided by the[JUnit plugin](https://plugins.jenkins.io/junit)for aggregating test reports. |

## Why Pipeline? {#why}

Jenkins is, fundamentally, an automation engine which supports a number of automation patterns. Pipeline adds a powerful set of automation tools onto Jenkins, supporting use cases that span from simple continuous integration to comprehensive continuous delivery pipelines. By modeling a series of related tasks, users can take advantage of the many features of Pipeline:

* **Code**: Pipelines are implemented in code and typically checked into source control, giving teams the ability to edit, review, and iterate upon their delivery pipeline.

* **Durable**: Pipelines can survive both planned and unplanned restarts of the Jenkins master.

* **Pausable**: Pipelines can optionally stop and wait for human input or approval before continuing the Pipeline run.

* **Versatile**: Pipelines support complex real-world continuous delivery requirements, including the ability to fork/join, loop, and perform work in parallel.

* **Extensible**: The Pipeline plugin supports custom extensions to its DSL\[[1](https://jenkins.io/doc/book/pipeline/#_footnote_1)\]and multiple options for integration with other plugins.

While Jenkins has always allowed rudimentary forms of chaining Freestyle Jobs together to perform sequential tasks,\[[2](https://jenkins.io/doc/book/pipeline/#_footnote_2)\]Pipeline makes this concept a first-class citizen in Jenkins.

Building on the core Jenkins value of extensibility, Pipeline is also extensible both by users with[Pipeline Shared Libraries](https://jenkins.io/doc/book/pipeline/shared-libraries/)and by plugin developers.\[[3](https://jenkins.io/doc/book/pipeline/#_footnote_3)\]

The flowchart below is an example of one continuous delivery scenario easily modeled in Jenkins Pipeline:

![](https://jenkins.io/doc/book/resources/pipeline/realworld-pipeline-flow.png "realworld pipeline flow")

Figure 1. Pipeline Flow

## Pipeline Terms {#terms}

Step

A single task; fundamentally steps tell Jenkins_what_to do. For example, to execute the shell command`make`use the`sh`step:`sh 'make'`. When a plugin extends the Pipeline DSL, that typically means the plugin has implemented a new_step_.

Node

Most_work_a Pipeline performs is done in the context of one or more declared`node`steps. Confining the work inside of a node step does two things:

1. Schedules the steps contained within the block to run by adding an item to the Jenkins queue. As soon as an executor is free on a node, the steps will run.

2. Creates a workspace \(a directory specific to that particular Pipeline\) where work can be done on files checked out from source control.




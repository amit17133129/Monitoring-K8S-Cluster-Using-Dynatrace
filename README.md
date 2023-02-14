# Monitoring-K8S-Cluster-Using-Dynatrace

<p align="center">
  <img width="1000" height="600" src="https://miro.medium.com/max/1100/0*iqw7hdugW42_KNC7.webp">
</p>

Hello guys, back with another article. In this article monitoring of kubernetes cluster is done using one of the most advanced monitoring tool i.e. dynatrace.

## What is Dynatrace?
Dynatrace is a software-intelligence monitoring platform that simplifies enterprise cloud complexity and accelerates digital transformation. With Davis (the Dynatrace AI causation engine) and complete automation, the Dynatrace all-in-one platform provides answers, not just data, about the performance of your applications, their underlying infrastructure, and the experience of your end users. Dynatrace is used to modernize and automate enterprise cloud operations, release higher-quality software faster, and deliver optimum digital experiences to your organization’s customers.


## How Dynatrace delivers its capabilities
Three patented technologies unique to Dynatrace dovetail with one another to enable automatic discovery, modeling, and analytics of each component and dependency across all tiers of your application. Dynatrace provides true full-stack monitoring.

OneAgent technology uses a single agent to collect and unify all operational and business performance metrics for all types of entities in your application environment — servers, applications, services, databases, and more — across each layer of your technology stack (including containers).

Smartscape visualization technology maps everything running in your environment and detects all causal dependencies between websites, applications, services, processes, hosts, networks, and cloud infrastructure.

Dynatrace patented PurePath® technology captures timings and code-level context for application transactions end to end, across all supported technologies, from cloud to mainframe.

## What can you do with Dynatrace?
Dynatrace is an all-in-one platform that’s purpose-built for a wide range of use cases.

- Infrastructure Monitoring. Dynatrace delivers simplified, automated infrastructure monitoring that provides broad visibility across your hosts, VMs, containers, network, events, and logs. Dynatrace continuously auto-discovers your dynamic environment and pulls infrastructure metrics into our Davis® AI engine, so you can consolidate tools and cut MTTI.

- Applications and Microservices. Dynatrace provides automated, code-level visibility and root-cause answers for applications that span complex enterprise cloud environments. Dynatrace automatically captures timing and code-level context for transactions across every tier. It also detects and monitors microservices automatically across the entire hybrid cloud, from mobile to mainframe.

- Application Security. Dynatrace enables you to deliver applications faster and more securely with automated runtime application vulnerability management, optimized for the cloud and Kubernetes.

- Digital Experience Monitoring (DEM). Dynatrace DEM provides Real User Monitoring (RUM) for every one of your customer’s journeys, synthetic monitoring across a global network, and 4K movie-like Session Replay. This powerful combination helps you optimize your applications, improve user experience, and provide superior support across all digital channels.

- Business Analytics. By tying business metrics and KPIs to data that’s already flowing through our application performance and digital experience modules, you get real-time, AI-powered answers to your critical business questions.

- Cloud Automation. Dynatrace AIOps gives you precise answers automatically. Dynatrace collects high-fidelity data and maps dependencies in real-time so that the Dynatrace explainable AI engine, Davis®, can show you the precise root causes of problems or anomalies, enabling quick auto-remediation and intelligent cloud orchestration.

So let’s start by integrating the kubernetes cluster with dynatrace. Create your free trial account in dynatrace. It will not ask you any debit or credit card. It will only ask you email to start monitoring with dynatrace. I am assuming that you already have kubernetes cluster ready. I am using here AWS EKS Cluster.

When you will login into dynatrace, click on deploy dynatrace in the left panel of the dynatrace webUI. Click on Install one Agent. It will take you in the below options.

<p align="center">
  <img width="1000" height="450" src="https://miro.medium.com/max/1100/1*DtlUly-fTlVIDjpJCrVFcA.webp">
</p>

Now after downloading the dynakube.yaml file run the commands as mentioned below. This is the same command you will be getting from the dynatrace webUI.

Login into your AWS EKS cluster and create a namespace with dynatrace. You use the below command to create the dynatrace namespace.

```
# Creating dynatrace namespace.

kubectl create namespace dynatrace
```

<p align="center">
  <img width="1000" height="200" src="https://miro.medium.com/max/1100/1*h6-Phd_x85lFOHXFtSbMCw.webp">
</p>

```
# Apply the dynatrace operator
kubectl apply -f https://github.com/Dynatrace/dynatrace-operator/releases/download/v0.10.2/kubernetes.yaml
```

<p align="center">
  <img width="1000" height="300" src="https://miro.medium.com/max/1100/1*vUcHYNZ4IqG7-Jie7Burbg.webp">
</p>

Now take the apiToken from the dynakube.yaml file and replace in the below command to create the secret.

```
kubectl -n dynatrace create secret generic dynakube --from-literal="apiToken="Enter_Token_Here"
```

<p align="center">
  <img width="1000" height="200" src="https://miro.medium.com/max/1100/1*qa6C9g9paQCN1DDuVlAfsA.webp">
</p>

Then apply the dynakube.yaml file in the AWS EKS cluster using below command.

```
kubectl apply -f dynakube.yaml
```

<p align="center">
  <img width="1000" height="70" src="https://miro.medium.com/max/1100/1*clUWnXBLI0hxxCxDYVzXeA.webp">
</p>

After this command the kubernetes cluster will be automatically integrated with dynatrace. Now navigate to dynatrace webUI and go to kubernetes section in the left pannel.

<p align="center">
  <img width="1000" height="300" src="https://miro.medium.com/max/1100/1*S5MkoqP0vW75ScQDi4BkbA.webp">
</p>

After sometime you can see that EKS cluster will be up and running in dynatrace webUI.

Click on the kubernetes cluster you will able to see the details of the cluster such as cores utilization, memory utilization, count of pods, namespaces, deployments etc.

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1100/1*YmWem6Y6h4yr_XPKdAzYPA.webp">
</p>

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1100/1*t6OIUg3NcYb8vSH8lfrlpg.webp">
</p>


If you move down to the same page you will able to see the detial sof the cluster utilization in the form of cpu usage, memory usgae

If you click on the kubernetes workload you will see that the total workload can be seen in that dashboard.

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1400/1*Os_9_duwI-Syz3-mzjxQBg.webp">
</p>

Navigate to host of the worker node, you will able to see the details of the worker node. It represents you cpu usage, memory usage, Traffic flow rate, system load etc.

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1100/1*PEHWsvdpbbMaTu-okzag2Q.webp">
</p>

Container analysis can be done by checking the container details in the host page itself.

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1100/1*IChVXfusM0SRJyVKgiBEfA.webp">
</p>

You can navigate to smartscape toplogy in the left pannel to see the more interactive graphs. You will find the details processes of each pod on each circle in below image.

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1100/1*p-QrZXVOO3WbSh0LXN8Yug.webp">
</p>

You can also check the details of the host. currently we have only two host as a worker node in the kubernetes cluster. You can see in the below interactive graph.

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1100/1*lnN-u9xA1EiDK_5rnQyXIg.webp">
</p>

## Creating dashboard:
Use Dynatrace dashboards and reports for quick, focused access to the data you need to monitor your environments.

To create the dashboard in dynatrace you will have to navigate to dashboard section in the left panel. There dynatrace will have pre-created dashboard. You can click on create new dashboard. Here i have created K8S-monitoring-dashboard. You can see the below gif to configure the graphs in the dashboard.

At the end after configuring the graphs in the dashboard it will look like below page. You add honeycomb graphs, set-top, pie charts, line-graphs, single value graphs, etc.

<p align="center">
  <img width="1000" height="550" src="https://miro.medium.com/max/1100/1*jSCGBLzKoCcJl3MxBdVs3w.webp">
</p>

I hope you had find this monitoring kubernetes cluster interesting. I hope this will help you to make your own dashboard and to do the monitoring kubernetes cluster.

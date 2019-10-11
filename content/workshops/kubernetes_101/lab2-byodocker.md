---
title: Lab 2 - BYO Docker
workshops: kubernetes_101
workshop_weight: 12
layout: lab
---

# Bring your own docker
It's easy to get started with Kubernetes whether you're using our app templates or bringing your existing docker assets.  In this quick lab we will deploy an application using an exisiting docker image.  Kubernetes will create an image stream for the image as well as deploy and manage containers based on that image.  And we will dig into the details to show how all that works.

## Let's point Kubernetes to an existing built docker image

{{< panel_group >}}
{{% panel "CLI Steps" %}}

<blockquote>
<i class="fa fa-terminal"></i> Goto the terminal and type the following:
</blockquote>
```bash
$ oc new-app mesosphere/kubernetes-guestbook-go
```

<blockquote>
The output should show something *similar* to below:
</blockquote>
```bash
--> Found Docker image ea8e3e2 (4 years old) from Docker Hub for "mesosphere/kubernetes-guestbook-go"
    * An image stream tag will be created as "kubernetes-guestbook-go:latest" that will track this image
    * This image will be deployed in deployment config "kubernetes-guestbook-go"
    * Port 3000/tcp will be load balanced by service "kubernetes-guestbook-go"
      * Other containers can access this service through the hostname "kubernetes-guestbook-go"
    * WARNING: Image "mesosphere/kubernetes-guestbook-go" runs as the 'root' user which may not be permitted by your cluster administrator
--> Creating resources ...
    imagestream.image.openshift.io "kubernetes-guestbook-go" created
    deploymentconfig.apps.openshift.io "kubernetes-guestbook-go" created
    service "kubernetes-guestbook-go" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/kubernetes-guestbook-go' 
    Run 'oc status' to view your app.
```

{{% /panel %}}

{{% panel "Web Console Steps" %}}

<blockquote>
Click "Add to Project"
</blockquote>
<img src="../images/ocp-addToProjectButton.png" width="150"><br/>

<blockquote>
Select the "Deploy Image" option from the drop down
</blockquote>
<img src="../images/ocp-guestbook-deploy-image.png" width="200"><br/>

<blockquote>
Select the option for "Image Name" and enter "mesosphere/kubernetes-guestbook-go", then click the magnifying glass to the far right to search for the image.
</blockquote>
<img src="../images/ocp-guestbook-imagename-expand2.png" width="600"><br/>

<blockquote>
Observe default values that are populated in the search results
</blockquote>
<img src="../images/ocp-guestbook-create-2.png" width="600"><br/>

<blockquote>
Click "Deploy" then click "Close"
</blockquote>

{{% /panel %}}
{{< /panel_group >}}


## We can browse our project details with the command line
> <i class="fa fa-terminal"></i> Try typing the following to see what is available to 'get':

```bash
$ oc get all
```

> <i class="fa fa-terminal"></i> Now let's look at what our image stream has in it:

```bash
$ oc get is
```
```bash
$ oc describe is/kubernetes-guestbook-go
```

{{% alert info %}}
An image stream can be used to automatically perform an action, such as updating a deployment, when a new image, in our case a new version of the guestbook image, is created.
{{% /alert %}}

> <i class="fa fa-terminal"></i> The app is running in a pod, let's look at that:

```bash
$ oc describe pods
```

## We can see those details using the web console too
Let's look at the image stream.  

<blockquote>
Click on "Builds -> Images"
</blockquote>

<img src="../images/ocp-guestbook-buildimages.png" width="200"><br/>

This shows a list of all image streams within the project.  

<blockquote>
Now click on the "kubernetes-guestbook-go" image stream
</blockquote>

You should see something similar to this:

<img src="../images/ocp-guestbook-is2.png" width="600"><br/>


## Does this kubernetes-guestbook-go do anything?
Good catch, your service is running but there is no way for users to access it yet.  We can fix that from the web console or the command line. You decide which you'd rather do from the steps below.

{{< panel_group >}}

{{% panel "CLI Steps" %}}

<blockquote>
<i class="fa fa-terminal"></i> In the command line type this:
</blockquote>

```bash
$ oc expose service kubernetes-guestbook-go
```
{{% /panel %}}

{{% panel "Web Console Steps" %}}

<blockquote>
"Overview"
</blockquote>

<img src="../images/ocp-guestbook-overview2.png" width="400"><br/>

<blockquote>
Select the arrow '>' next to 'kubernetes-guestbook-go, #' 
</blockquote>

<img src="../images/ocp-kubernetes-arrow3.png" width="400"><br/>


<blockquote>
To get to this view
</blockquote>

<img src="../images/ocp-guestbook-noroute2.png" width="600"><br/>

<p>Notice there is no exposed route </p>

<blockquote>
Click on the "Create Route" link
</blockquote>

<img src="../images/ocp-guestbook-createRoute2.png" width="600"><br/>

<p>This is where you could specify route parameters, but we will just use the defaults.</p>

<blockquote>
Click "Create"
</blockquote>

{{% /panel %}}
{{< /panel_group >}}

{{% alert info %}}
You can also create secured HTTPS routes, but that's a topic for a more advanced workshop
{{% /alert %}}

## Test out the guestbook webapp
Notice that in the web console overview, you now have a URL in the service box.  There is no database setup, but you can see the webapp running by clicking the route you just exposed.

> Click the link in the service box. You should see:

<img src="../images/ocp-guestbook-app.png" width="600"><br/>


## Let's clean this up

> <i class="fa fa-terminal"></i> Let's clean up all this to get ready for the next lab:

```bash
$ oc delete all --all
```


# Summary
In this lab you've deployed an example docker image, pulled from docker hub, into a pod running in Kubernetes.  You exposed a route for clients to access that service via thier web browsers.  And you learned how to get and describe resources using the command line and the web console.  Hopefully, this basic lab also helped to get you familiar with using the CLI and navigating within the web console.

{{< importPartial "footer/footer.html" >}}

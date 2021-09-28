<h1>Create and Manage Cloud Resources: Challenge Lab</h1>

1. Open Google Console 
2. Login
3. Open Cloud Shell
4. Enter the following commands: <br>
   - gcloud auth list <br>
   - gcloud config list project
 
Topics tested in this Challenge:
- Create an instance
- Create a 3-node Kubernetes cluster and run a simple service
- Create an HTTP(s) load balancer in front of two web servers

<hr>

<h3>Task 1 : Create a project jumphost instance</h3>
You will use this instance to perform maintenance for the project.

Requirements:

- Name the instance nucleus-jumphost.
- Use an f1-micro machine type.
- Use the default image type (Debian Linux).

Solution: 
```
gcloud compute instances create nucleus-jumphost \
          --network nucleus-vpc \
          --zone us-east1-b  \
          --machine-type f1-micro  \
          --image-family debian-9  \
          --image-project debian-cloud
```
<hr>

<h3>Task 2 : Create a Kubernetes service cluster</h3>

The team is building an application that will use a service running on Kubernetes. 

You need to:

- Create a cluster (in the us-east1-b zone) to host the service.
- Use the Docker container hello-app (gcr.io/google-samples/hello-app:2.0) as a place holder; the team will replace the container with their own work later.
- Expose the app on port 8080.

Solution: 
```
gcloud container clusters create nucleus-backend \
          --num-nodes 1 \
          --network nucleus-vpc \
          --region us-east1
```
 
```
gcloud container clusters get-credentials nucleus-backend \
          --region us-east1
```

```
kubectl create deployment hello-server \
          --image=gcr.io/google-samples/hello-app:2.0
```

```
kubectl expose deployment hello-server \
          --type=LoadBalancer \
          --port 8080
```

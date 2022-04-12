Example Voting App
=========

A simple distributed application running across multiple Docker containers.


Architecture
-----

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) or [ASP.NET Core](/vote/dotnet) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) or [NATS](https://hub.docker.com/_/nats/) queue which collects new votes
* A [.NET Core](/worker/src/Worker), [Java](/worker/src/main) or [.NET Core 2.1](/worker/dotnet) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) or [TiDB](https://hub.docker.com/r/dockersamples/tidb/tags/) database backed by a Docker volume
* A [Node.js](/result) or [ASP.NET Core SignalR](/result/dotnet) webapp which shows the results of the voting in real time

How to do it via docker way
-----

docker run -d --name=redis redis
docker run -d --name=db postgres:9.4
docker run -d --name=vote -p 5000:80 voting-app --link redis:redis
docker run -d --name=result -p 5001:80 result-app --link db:db
docker run -d --name=worker worker

Kubernetes Way
-----

Goals:
1. Deploy containers
2. Enable connectivity
3. External Access

Steps:
1. Deploy PODs(Total 5 Pods)
2. Create Service(ClusterIP)
    1. redis
    2. db(postgres)
3. Create Service (NodePort)
    1. voting-app
    2. result-app
4. Create Deployment (for rolling update and replicas)

* kubectl create -f "all pods and services"
* minikube service voting-service --url

Setup Multi Node CLuster using kubeadm
-----

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

Steps:

1. Designate one node as master other as worker
2. Install docker on all nodes
3. kubeadm
4. Initialize
5. Pod network
6. join worker node to master node









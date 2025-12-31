GitOps Deployment with Argo CD on Minikube

This project shows how to deploy an application to Kubernetes using GitOps — meaning:

✅ Application configuration lives in Git
✅ Argo CD watches the Git repo
✅ When files in Git change → the cluster updates automatically
❌ No manual kubectl apply needed

This demo runs completely locally using:

Minikube — local Kubernetes cluster

Argo CD — GitOps deployment tool

GitHub — stores Kubernetes manifests

🌍 What is GitOps? (Simple Explanation)

GitOps means:

“Git is the single source of truth for deployments.”

So instead of deploying apps by hand, you:

Push Kubernetes YAML files to Git

Argo CD reads them

Argo CD deploys them automatically

Any new commit triggers an update

This is how modern DevOps teams work.

🏗 Project Overview

This repo contains Kubernetes manifests for a sample app:

k8s/
 ├── deployment.yaml   → defines the app
 └── service.yaml      → exposes the app


Argo CD is configured to watch the k8s/ folder in this repo.

If you change the image version in deployment.yaml and push to Git,
Argo CD will:

✔ detect the change
✔ sync the cluster
✔ rollout the update automatically

That’s GitOps 🎉

🚀 How This Was Set Up (High-Level)

Start Minikube

Install Argo CD in Kubernetes

Expose the Argo CD UI

Login to Argo CD

Create an Application in Argo CD that points to this repo

Make a change in Git — watch auto-deployment happen

So anyone can repeat this locally.

📦 Kubernetes Resources
Deployment

Defines the app + image:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: demo
        image: nginx:1.26   # ← change version here to trigger update
        ports:
        - containerPort: 80

Service

Exposes the app:

apiVersion: v1
kind: Service
metadata:
  name: demo-service
spec:
  selector:
    app: demo-app
  ports:
  - port: 80
    targetPort: 80
  type: NodePort

🔄 Testing the GitOps Flow
Step-1 — Change the image in Git

Example:

nginx:1.26 → nginx:1.27


Commit & push.

Step-2 — Argo CD detects change

UI shows:

🟠 OutOfSync → 🟢 Synced

Step-3 — Kubernetes redeploys pod

You can verify with:

kubectl describe deployment demo-app


Look for:

Image: nginx:1.27


🎯 Done — deployment updated through Git

🧠 Why This Matters

This demo shows how GitOps provides:

✔ Version-controlled deployments
✔ Audit history
✔ Auto-rollouts & rollbacks
✔ Safer deployments
✔ Fully automated sync

Exactly how real DevOps teams work.

🛠 Requirements

Docker

Minikube

kubectl

Git

Browser (for Argo CD UI)

🩺 Troubleshooting
Issue	Fix
App not updating	Check Argo CD sync status
Pod stuck	kubectl describe pod
Service unreachable	Ensure pod is Running
Image not pulled	Use public images
✅ Project Status

This project successfully demonstrates:

✔ Git-driven Kubernetes deployments
✔ Argo CD continuous sync
✔ Automatic rollout on commit
✔ Local GitOps workflow

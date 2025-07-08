# Kubernetes Deployment Commands

This guide documents common `kubectl` commands used to manage a Kubernetes deployment using the example application `hello-world-rest-api`.

---

## 📦 1. Create a Deployment

Command: `kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE`

- Creates a Deployment with the specified Docker image.
- Kubernetes automatically creates a ReplicaSet and Pod to run the container.

---

## 🌐 2. Expose the Deployment

Command: `kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080`

- Exposes the Deployment as a Kubernetes Service.
- `--type=LoadBalancer`: Makes the service externally accessible (typically used in cloud environments).
- `--port=8080`: The port on which the service will be exposed.

---

## 📈 3. Manually Scale the Deployment

Command: `kubectl scale deployment hello-world-rest-api --replicas=3`

- Scales the Deployment to **3 replicas** (Pods).
- The ReplicaSet ensures the desired number of pods is maintained.

---

## ⚙️ 4. Delete a Pod (Self-Healing Demo)

Command: `kubectl delete pod hello-world-rest-api-58ff5dd898-6219d`

- Deletes a specific pod.
- Kubernetes automatically creates a replacement pod to maintain the desired state defined in the Deployment.

---

## 🔁 5. Update Deployment Image (Rolling Update)

Command: `kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE`

- Updates the container image version in the Deployment.
- Kubernetes performs a **rolling update**:
  - A new ReplicaSet is created with the new image.
  - It gradually increases the number of new Pods and decreases the old ones.
  - Ensures zero downtime during updates.

---

## 🤖 6. Enable Horizontal Pod Autoscaling (HPA)

Command: `kubectl autoscale deployment hello-world-rest-api --min=1 --max=10 --cpu-percent=70`

- Enables autoscaling based on CPU usage.
- Automatically adjusts the number of Pods between **1 and 10**.
- Creates a `HorizontalPodAutoscaler` resource.

To check the autoscaler: `kubectl get hpa`

---

## 🚀 7. Zero Downtime Deployment with minReadySeconds

1. Edit the deployment configuration: `kubectl edit deployment hello-world-rest-api`
2. Under the `spec` section, add: `minReadySeconds: 15`
3. Save and exit the editor.
4. Trigger a new rolling update: `kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE`

- `minReadySeconds` ensures the new pod remains in the "Ready" state for at least 15 seconds before terminating old pods.
- This enables **zero downtime deployments**.

---

> ✅ Keep experimenting with scaling, updating, and autoscaling to better understand how Kubernetes ensures high availability and fault tolerance.

---

# 🔧 Common Kubernetes (`kubectl`) Commands

A quick reference guide to frequently used `kubectl` commands to inspect and manage resources in a Kubernetes cluster.

---

## 🔍 1. Check Client and Server Version

- `kubectl version`

Displays the version info for both the client and the Kubernetes server.

---

## 📅 2. View Cluster Events

- `kubectl get events`
- `kubectl get events --sort-by=.metadata.creationTimestamp`

View events occurring in the cluster. Sorting helps to see the most recent events first.

---

## 🧪 3. Work with Pods

- `kubectl get pods` — List all pods in the current namespace.
- `kubectl get pods -o wide` — Show additional details like node and IP.
- `kubectl get po` — Alias for `kubectl get pods`.
- `kubectl describe pod <POD_NAME>` — Detailed info about a specific pod.
- `kubectl explain pods` — Describes the API schema for pods.

---

## 📦 4. Manage ReplicaSets

- `kubectl get replicaset`
- `kubectl get replicasets`
- `kubectl get rs` — Alias for replicaset.
- `kubectl get replicaset -o wide` / `kubectl get rs -o wide` — Extended output.
- `kubectl explain replicaset` — View detailed schema and fields.

---

## 🚀 5. View Deployments

- `kubectl get deployment`

Lists all deployments in the current namespace.

---

## 🌐 6. View Services

- `kubectl get services`
- `kubectl get svc` — Alias for services.
- `kubectl get svc --watch` — Continuously watch service changes in real-time.

---

## 📋 7. View All Resources

- `kubectl get all`

Displays pods, services, deployments, ReplicaSets, and more in one command.

---

## 📈 8. View Horizontal Pod Autoscaler (HPA)

- `kubectl get hpa`

Check status of autoscalers created via `kubectl autoscale`.

---

## 📊 9. View Pod Resource Usage

- `kubectl top pod`

Shows CPU and memory usage of all pods. Requires `metrics-server` to be installed.

---

## 🖥️ 10. View Node Resource Usage

- `kubectl top nodes`

Displays CPU and memory usage per node. Requires `metrics-server`.

---

## ❌ 11. Delete Horizontal Pod Autoscaler

- `kubectl delete hpa currency-exchange`

Deletes a specific HPA resource named `currency-exchange`.

---

> 💡 Use `-o wide` to get more context. Use `--watch` to observe changes in real-time. Aliases like `po`, `rs`, and `svc` can speed up your workflow.

---

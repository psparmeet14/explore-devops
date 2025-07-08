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

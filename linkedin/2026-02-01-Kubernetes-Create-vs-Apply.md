I didn't understand the difference between kubectl create and kubectl apply until I saw this.

When I first started working with Kubernetes manifests, I thought both commands did the same thing. Create a resource from a YAML file, right?

Wrong.

Here's what actually happens:

kubectl create:
- Only creates the resource
- Fails if the resource already exists
- One-way operation

kubectl apply:
- Creates the resource if it doesn't exist
- Detects changes between your manifest and existing resource
- Intelligently updates only what changed

The moment this clicked for me was when I edited my nginx.yaml file to add another label.

First run: kubectl apply -f nginx.yaml
Output: pod/nginx-yaml created

After editing the file: kubectl apply -f nginx.yaml
Output: pod/nginx-yaml configured

Notice the output changed from "created" to "configured". Apply detected the differences and updated the pod accordingly.

This is how production teams manage Kubernetes. Not one-off creates, but declarative applies that track changes over time.

Your YAML manifest becomes the source of truth. Apply it repeatedly. Kubernetes handles the rest.

Have you been using create or apply? What made you switch? 👇

#Kubernetes #DevOps #KubeCraft #LearningInPublic #CloudNative

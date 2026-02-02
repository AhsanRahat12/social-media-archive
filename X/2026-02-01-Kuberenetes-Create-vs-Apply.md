kubectl create vs kubectl apply. Here's the difference that matters:

create: Only creates. Fails if resource exists.

apply: Creates OR updates. Detects changes intelligently.

Example:
First run: kubectl apply -f nginx.yaml
→ Output: "pod/nginx-yaml created"

After editing file: kubectl apply -f nginx.yaml
→ Output: "pod/nginx-yaml configured"

Apply detected the changes and updated accordingly.

This is how production teams work. Your YAML is the source of truth. Apply repeatedly.

#Kubernetes #DevOps #CloudNative

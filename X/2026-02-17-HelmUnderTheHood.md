Helm isn't magic. It's smart automation over kubectl.

What happens during `helm install`:

✅ Reads chart's default values.yaml
✅ Merges with your custom values  
✅ Processes templates directory
✅ Substitutes {{ .Values.replicaCount }} with actual values
✅ Generates Kubernetes YAML manifests
✅ Runs kubectl apply under the hood

The templates are just regular K8s manifests with variables instead of hardcoded values.

When I set replicaCount: 2, Helm finds:
replicas: {{ .Values.replicaCount }}

And generates:
replicas: 2

That's it. Variable substitution. Then kubectl apply.

Helm charts aren't mysterious. They're organized collections of YAML we already know, made parameterizable for reuse across environments.

Learning kubectl first was essential. Without that foundation, Helm is black box magic. With it, Helm is a logical next step.

#DevOps #Kubernetes #Helm

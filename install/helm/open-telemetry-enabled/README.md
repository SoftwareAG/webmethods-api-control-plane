# API Control Plane helm chart

This helm chart installs Control Plane product in a k8s cluster wiht Open telemetry enabled via Jaegar. 
Refer `values.yaml` for the configurable values that can be passed while running the helm installation command.

> **Note:** The machine needs to have kubernetes and helm as a prerequisite

You can run the below command to install the product in a namespace `control-plane`.

```
$ helm upgrade \
    --install \
    --create-namespace \
    --namespace control-plane \
    --wait \
    --timeout 5m0s \
    control-plane \
    . \
	--set applications.jaegertracing.extPort=3007 \
    --set ingressHost=control-plane
	
```

> **Note:** The `ingressHost` should be added to `/etc/hosts` (for linux) or `C:\Windows\System32\drivers\etc\hosts` (for windows)
pointing to the VM where the k8s cluster is running to access the product from the developer machine.

> **Note:** The Jaeger UI can be accessed via the applications.jaegertracing.extPort property which will be configured as a Node port

> **Note:** The Jaeger provided in this sample is a simple deployment hence is stateless and does not uses any volume mapping
Please refer to  https://github.com/hansehe/jaeger-all-in-one/tree/master/helm to configure Jaeger as a Stateful set with property
volume mappings if needed.
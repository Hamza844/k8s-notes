# Kubernetes Command and Tips ☸️

## Insatllation of metric for CPU :

- If you are using a Kind cluster install Metrics Server
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
- Edit the Metrics Server Deployment
```bash
kubectl -n kube-system edit deployment metrics-server
```
- Add the security bypass to deployment under `container.args`
```bash
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
```
- Restart the deployment
```bash
kubectl -n kube-system rollout restart deployment metrics-server
```
## Generate Load Using BusyBox
- Run this command to create a temporary BusyBox pod in the dev namespace:

```bash
kubectl run -n dev busybox --image=busybox --restart=Never --rm -it -- /bin/sh
```
- Once inside the shell (you’ll see a prompt like / #), run this infinite loop to generate CPU load on Apache:
```bash
while true; do wget -q -O- http://apache.dev.svc.cluster.local; done
```

✅ Step 1: Install Vertical Pod Autoscaler (VPA) Components

- Install the VPA components (CRDs + controller pods) in your cluster.
```bash
git clone https://github.com/kubernetes/autoscaler.git
cd autoscaler/vertical-pod-autoscaler/
./hack/vpa-up.sh
```
- Verify the installation by checking for CRDs and pods:

```bash
kubectl get crd | grep verticalpodautoscalers
kubectl get pods -n kube-system | grep vpa
```

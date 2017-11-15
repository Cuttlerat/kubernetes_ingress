# Deploying the Nginx instance

Now we've allowed services to run on a NodePort of 80 we can deploy the Nginx service! This will setup a default Nginx ingress based on: https://github.com/kubernetes/ingress-nginx/blob/master/deploy/README.md.

Execute the following commands, this will setup a new namespace ingress-nginx and setup the Nginx default config and a default HTTP backend in this namespace.

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/namespace.yaml | kubectl apply -f -
```

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/default-backend.yaml | kubectl apply -f -
```

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/configmap.yaml | kubectl apply -f -
```

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/tcp-services-configmap.yaml | kubectl apply -f -
```

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/udp-services-configmap.yaml | kubectl apply -f -
```

If you have RBAC setup on your cluster (most versions of Kubernetes >=1.7 do) then execute the following commands to deploy the Nginx instance:

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/rbac.yaml | kubectl apply -f -
```

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/with-rbac.yaml | kubectl apply -f -
```

For non-RBAC execute the following command:

```bash
curl https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/without-rbac.yaml | kubectl apply -f -
```

# Deploying the Nginx service

Now the most important step, this is my custom config which will allow you to expose the Nginx instance on port 80.

Save the following to a file nginx-service.yml

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: ingress-nginx
  namespace: ingress-nginx
spec:
  type: NodePort
  ports:
  - name: http
    port: 80
    targetPort: 80
    nodePort: 80
    protocol: TCP
  selector:
    app: ingress-nginx
```
Apply this to the cluster with kubectl `apply -f nginx-service.yml`

Copypast from http://will3942.com/nginx-kubernetes-bare-metal

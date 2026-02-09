## What You’ll Deploy
* A Deployment (2 NGINX Pods)
* A ConfigMap (stores config)
* A Secret (stores credentials)
* Liveness & Readiness Probes
* A NodePort Service (makes it accessible)

### Let’s go step-by-step:

#### Step 1: ConfigMap
configmap.yaml
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: "production"
Injects config like APP_MODE=production.
```

#### Step 2: Secret
secret.yaml
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  username: YWRtaW4=         # admin
  password: c2VjdXJlcGFzcw== # securepass
Injects credentials securely using environment variables.
```

#### Step 3: Deployment with Probes
deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.21
          ports:
            - containerPort: 80
          envFrom:
            - configMapRef:
                name: app-config
          env:
            - name: ADMIN_USER
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: username
            - name: ADMIN_PASS
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: password
          livenessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 10
            failureThreshold: 3
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            failureThreshold: 2
```

#### Step 4: Service (NodePort)
service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: NodePort
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```
## 📌 Access your app via:
http://<Node-IP>:30080

Quick Commands to Deploy It All
# Apply the ConfigMap definition
kubectl apply -f configmap.yaml

# Apply the Secret definition
kubectl apply -f secret.yaml

# Deploy your application (creates Pods via Deployment)
kubectl apply -f deployment.yaml

# Optional: Watch Pod status to confirm they start successfully
kubectl get pods -w

# Optional: If issues, debug a Pod
kubectl describe pod <pod-name>
kubectl logs <pod-name>

# Apply Service to expose your app
kubectl apply -f service.yaml

# View service details (e.g., NodePort)
kubectl get service web-service

# Optional: Access the app via port-forwarding (if NodePort not reachable)
kubectl port-forward service/web-service 8080:80

# Optional: Check env vars injected into the Pod (verify config/secret)
kubectl exec -it <pod-name> -- env

# List all running Pods in the current namespace
kubectl get pods


- Implement the Backing Service
- Set up the kubernetes deployment
- Perform a rolling update

#### Implement the Backing Service
The application relies on a backing web service. The Pod(s) for the backing service
are already running in the cluster, and they have the label app=pirate-day-backend. 
The backing service application listens on port 3000. Create a NodePort
Service called pirate-day-backend-svc for this backing service with the nodePort 30081.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: pirate-day-backend-svc
spec:
  type: NodePort
  selector:
    app: pirate-day-backend
  ports:
  - protocol: TCP
    port: 3000
    targetPort: 3000
    nodePort: 30081
```

Create a Deployment called pirate-day-dep to run the app with 3 replicas. 
Include the label app=pirate-day-frontend on each Pod. An existing NodePort
Service will allow access to the app using this label at http://<K8s_SERVER_PUBLIC_IP>:30080.
Use the image linuxacademycontent/pirate-day:0.0.2.
A ConfigMap already exists in the cluster called pirate-config. 
This ConfigMap contains a top-level key called .env that represents the configuration file for the app. Mount this config data as a file to the containers at /usr/src/app/.env. This will ensure the frontend points to the correct URL for the backing service.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pirate-day-dep
spec:
  replicas: 3
  selector:
    matchLabels:
      app: pirate-day-frontend
  template:
    metadata:
      labels:
        app: pirate-day-frontend
    spec:
      containers:
      - name: pirate-day
        image: linuxacademycontent/pirate-day:0.0.2
        ports:
        - containerPort: 3000
        volumeMounts:
        - name: pirate-config
          mountPath: /usr/src/app/.env
          subPath: .env
      volumes:
      - name: pirate-config
        configMap:
          name: pirate-config
```


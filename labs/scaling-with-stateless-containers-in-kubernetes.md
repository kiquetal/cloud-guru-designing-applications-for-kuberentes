Stateless applications are a great tool to help facilitate robustness 
and high availability. In this lab, you will get hands-on with 
stateless containers. You will have the opportunity to design an 
application with stateless containers, and you will be able to see how easy 
it is to scale stateless applications in Kubernetes.

-Configure the App's Containers to be Stateless

Make the container file system for the app's containers read-only.
The application needs to be able to write to /tmp. 
You will need to create an external volume to allow the application
to write to this directory at runtime.
You can find a deployment descriptor for the app at /home/cloud_user/pirate-day.yml.


````yaml
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
        - name: tmp
          mountPath: /tmp
          readOnly: false
      volumes:
      - name: pirate-config
        configMap:
          name: pirate-config
      - name: tmp
        emptyDir: {}
    
````
- Scale the Application Up.



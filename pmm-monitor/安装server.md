##### pmm-server

官方提供一个docker image，```percona/pmm-server:latest```该镜像集成pmm-server所有的组件,优点是安装简单，确定是不利于扩展，

这里我们采用kubernetes 来安装

pmm.yaml

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  namespace: devops
  name: mysql-monitor
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: mysql-monitor
        role: monitor
    spec:
      containers:
      - name: mysql-monitor
        image: registry.boyait.9fwealth.com:1443/devops/images/pmm-server:1.4
        imagePullPolicy: Always
        resources:
          requests:
            cpu: 1000m
            memory: 2048Mi
        ports:
        - containerPort: 8500 #consul
        - containerPort: 80 # ui
        - containerPort: 9090 # prometheus
      imagePullSecrets:
        - name: bycx-registry

```

svc.yaml

```yaml
apiVersion: v1
kind: "Service"
metadata:
  name: mysql-ui
  namespace: devops
  labels:
    name: mysql-ui
spec:
  ports:
    - name: mysql-ui
      protocol: TCP
      port: 80
      targetPort: 80
  selector:
    app: mysql-monitor


---

apiVersion: v1
kind: "Service"
metadata:
  name: mysql-prom
  namespace: devops
  labels:
    name: mysql-prom
spec:
  ports:
    - name: mysql-prom
      protocol: TCP
      port: 9090
      targetPort: 9090
  selector:
    app: mysql-monitor


---

apiVersion: v1
kind: "Service"
metadata:
  name: mysql-consul
  namespace: devops
  labels:
    name: grafana-service
spec:
  ports:
    - name: mysql-consul
      protocol: TCP
      port: 8500
      targetPort: 8500
  selector:
    app: mysql-monitor
```

ingress.yaml

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: mysql-monitor
  namespace: devops
  annotations:
    nginx.ingress.kubernetes.io/affinity: cookie
    nginx.ingress.kubernetes.io/session-cookie-name: INGRESSCOOKIE
    nginx.ingress.kubernetes.io/session-cookie-hash: sha1
    kubernetes.io/ingress.class: "nginx-port-80-443"
spec:
  rules:
  - host: mysql-consul.hd1pro.boyacx.com
    http:
      paths:
      - backend:
          serviceName: mysql-consul
          servicePort: 8500
        path: /
  - host: mysql-ui.hd1pro.boyacx.com
    http:
      paths:
      - backend:
          serviceName: mysql-ui
          servicePort: 80
        path: /
  - host: mysql-prom.hd1pro.boyacx.com
    http:
      paths:
      - backend:
          serviceName: mysql-prom
          servicePort: 9090
        path: /
```



 ```yaml
kubectl create -f .
 ```


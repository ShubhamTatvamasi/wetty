# wetty

create a deployment and service:
```bash
kubectl create deployment wetty --image=wettyoss/wetty
kubectl expose deployment wetty --port=3000 --name=wetty
```

Ingress value for wetty:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: wetty
  annotations:
    nginx.org/websocket-services: wetty
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
      - wetty.k8s.shubhamtatvamasi.com
    secretName: wetty-tls
  rules:
  - host: wetty.k8s.shubhamtatvamasi.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: wetty
            port:
              number: 3000
EOF
```

https://wetty.k8s.shubhamtatvamasi.com/wetty

create a new user:
```bash
kubectl exec -it deploy/wetty -- adduser shubham
```

delete everything:
```bash
kubectl delete deploy/wetty svc/wetty ing/wetty
```

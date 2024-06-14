

apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress
  namespace: nginx-ingress
  alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}, {"HTTP": 8080}, {"HTTPS": 8443}] 
  alb.ingress.kubernetes.io/load-balancer-name: custom-name
  alb.ingress.kubernetes.io/target-type: instance
  alb.ingress.kubernetes.io/scheme: internal  / internet-facing
spec:
  type: Loadbalancer
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https
  selector:
    app: nginx-ingress










Internet -> DNS -> LB [10.10.10.10 ] -> InfraNode / Worker:NodePort[80,443] -> Proxy / Calico [POD, SVC ClusterIP] -> Iptables & Route -> ClusterIP -> nginx-ingress -> POD [192.168.235.11 ] -> IngressRules -> hello-python ClusterIP 10.96.10.51 : 30007 -> 192.168.235.17:80 



ingressClass: internal / external 
host: python 
path: / 
  service : hello-python 
  port: 30007 




kubectl apply -f 01-IngressCantroller.yaml 

kubectl get svc,pods -n nginx-ingress-internal  
pod-ingress-xkjhjashdjhas 192.168.235.11 
pod-ingress-xkjhjashdjhaw 192.168.235.12 
pod-ingress-xkjhjashdjhax 192.168.235.14 

svc 
nginx-ingress Loadbalancer 10.96.10.11 10.10.10.10   80:80, 443:443 




kubectl get svc,pods -n nginx-ingress-external  
pod-ingress-xkjhjashdjhas 192.168.235.11 
pod-ingress-xkjhjashdjhaw 192.168.235.12 
pod-ingress-xkjhjashdjhax 192.168.235.14 

svc 
nginx-ingress Loadbalancer 10.96.10.11 10.10.10.10   80:80, 443:443 




kubectl get svc,pods -n python  
pod-python-xkjhjashdjhas 192.168.235.17 
pod-python-xkjhjashdjhaw 192.168.235.18 
pod-python-xkjhjashdjhax 192.168.235.19 

svc 
hello-python ClusterIP 10.96.10.51   <none>  30007

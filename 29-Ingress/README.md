```
1305  cat 04-ingress-rules.yaml
 1306  kubectl  get svc
 1307  kubectl  get ingress
 1308  kubectl  delete ingres cafe-ingress
 1309  kubectl  delete ingress cafe-ingress
 1310  kubectl  get ingress
 1311  kubectl  describe ingress
 1312  kubectl get pods -o wide
 1313  curl 172.31.0.102:32519
 1314  curl 172.31.0.102:32519 -H 'Host: helloworld-v1.example.com'
 1315  curl 172.31.0.102:32519/info -H 'Host: helloworld-v1.example.com'

```

```
1327  kubectl apply -f 01-cafe.yaml
 1328  kubectl apply -f 02-cafe-ingress-2.yaml
 1329  kubectl  get pods
 1330  kubectl  get ingress
 1331  kubectl describe ingress cafe-ingress
 1332  curl 172.31.0.102:32519 -H 'Host: cafe.example.com'
 1333  curl 172.31.0.102:32519/tea -H 'Host: cafe.example.com'
 1334  kubectl  get pods -o wide
 1335  curl 172.31.0.102:32519/tea -H 'Host: cafe.example.com'
 1336  curl 172.31.0.102:32519/coffee -H 'Host: cafe.example.com'

```

```
Example2 

1370  kubectl  get ingress
 1371  curl 172.31.0.102:32519/tea -H 'Host: cafe.example.com'
 1372  curl 172.31.0.102:32519/coffee -H 'Host: cafe.example.com'
 1373  curl --resolve cafe.example.com:30085:172.31.0.102 https://cafe.example.com:30085/coffee --insecure
 1374  ls
 1375  cd ..
 1376  ls
 1377  cat README.md
 1378  curl --resolve cafe.example.com:31247:172.31.0.102 https://cafe.example.com:31247/coffee --insecure
 1379  curl --resolve cafe.example.com:31247:172.31.0.102 https://cafe.example.com:31247/tea --insecure

```

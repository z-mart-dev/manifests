Taken pretty much verbatim from here: https://kubernetes.github.io/ingress-nginx/deploy/#digital-ocean

Peculiar thing: The load balancer will run a health check at the NodePort on the ingress-nginx-controller at the http:80
destination, but for some reason that only seems to run/live on one node. It turns out a HealthCheck NodePort is created,
and works, but the load balancer will need to be manually updated run health checks on that.

```yaml
➜ ~ kubectl -n ingress-nginx describe svc ingress-nginx-controller
Name:                     ingress-nginx-controller
Namespace:                ingress-nginx
# [blah blah blah..]
Port:                     http  80/TCP
TargetPort:               http/TCP
NodePort:                 http  31869/TCP # <= LB Health check targets this
Endpoints:                10.244.3.183:80
Port:                     https  443/TCP
TargetPort:               https/TCP
NodePort:                 https  31015/TCP
Endpoints:                10.244.3.183:443
Session Affinity:         None
External Traffic Policy:  Local
HealthCheck NodePort:     31387 # <= LB Health check should target that
```

More weirdness: pod communication through the loadbalancer:

https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nginx-ingress-with-cert-manager-on-digitalocean-kubernetes#step-2-%E2%80%94-setting-up-the-kubernetes-nginx-ingress-controller

More docs:
https://docs.digitalocean.com/products/kubernetes/how-to/configure-load-balancers/

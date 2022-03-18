# Istio Commands Utils

### Adding a MemberRoll
```console
$ oc patch smmr default -n Istio-system --type='json' -p '[{"op": "add", "path": "/spec/members", "value":["'"mynamespace"'"]}]'
```

### Enabling sidecard
```console
$ oc patch dc/customer -p '{"spec":{"template":{"metadata":{"annotations":{"sidecar.istio.io/inject":"true"}}}}}' -n $OCP_NS
```
### Enabling sidecard in differents deploys
```console
$ for p in $(oc get deploy -o custom-columns=SERVICES:.metadata.name);
    do oc patch deploy/$p -p '{"spec":{"template":{"metadata":{"annotations":{"sidecar.istio.io/inject":"true"}}}}}'; 
  done
```
### Keep route working of Non-mesh services that need to be deployed within a service mesh enlisted namespace
```console
oc patch deployment messager-backend -p '{"spec":{"template":{"metadata":{"labels":{"maistra.io/expose-route":"true"}}}}}'
```
### Trigger a rollout for deployment
```console
    oc patch deployment/<deployment> -p '{"spec":{"template":{"metadata":{"annotations":{"kubectl.kubernetes.io/restartedAt": "'`date -Iseconds`'"}}}}}'
```

### Project to be used as sample
```console
oc new-app -l app=customer,version=v1 --name=customer --docker-image=quay.io/rcarrata/customer:quarkus -e VERSION=v1 -e  JAVA_OPTIONS='-Xms512m -Xmx512m -Djava.net.preferIPv4Stack=true'

oc new-app -l app=partner,version=v1 --name=partner --docker-image=quay.io/rcarrata/partner:sb -e JAVA_OPTIONS='-Xms512m -Xmx512m -Djava.net.preferIPv4Stack=true' 

oc new-app -l app=preference,version=v1 --name=preference --docker-image=quay.io/rcarrata/preference:quarkus -e JAVA_OPTIONS='-Xms512m -Xmx512m -Djava.net.preferIPv4Stack=true'

oc new-app -l app=recommendation,version=v1 --name=recommendation --docker-image=quay.io/dsanchor/recommendation:vertx -e JAVA_OPTIONS='-Xms512m -Xmx512m -Djava.net.preferIPv4Stack=true' -e VERSION=v1
```



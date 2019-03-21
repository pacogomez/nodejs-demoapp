# Source-to-Service Borathon Demo

# steps

1. edit code and push to repository
2. publish to Knative
3. register service with CSP
4. use service
5. iterate 1-2



# scripts
```
# edit code and push to repository
vi views/index.ejs

git commit -m 'update UI' -a

git push


# publish to Knative

# configure  credentials with docker hub
export KNCTL_NAMESPACE=helloworld
knctl basic-auth-secret create -s registry --docker-hub -u -p
knctl service-account create --service-account build -s registry

knctl deploy --service demoapp \
  --git-url https://github.com/pacogomez/nodejs-demoapp.git \
  --git-revision master --service-account build \
  --image index.docker.io/pacogomez/demoapp

# sanity check, get host and port of the service
knctl service list
knctl service show --service demoapp
knctl curl --namespace helloworld --service demoapp

# configure default route to service in istio
kubectl apply -f s2s-demo-route.yaml -n helloworld

#
export CSPI_PROFILE=stg_s2s

cspi create service --file s2s.yaml

```

#### ArgoEvents

#### Prereqs

- minikube
- deploy ArgoEvents
- deploy Argo Event Bus
- Github PAT (with webhook event enabled)

---

#### Github as source of events

The file `github-event-src.yaml` contains EventSource and Sensor.

The EventSource is github webhook: GH Repo -> Settings -> Webhook

As the WebHook url put ngrok url: `ngrok http http://localhost:12000` (note: port 12000 comes from the service prop in EventSource)

Also you would need to create k8s secret(populate token and secret files with PAT and random secret):
```sh
kubectl create secret generic github-pat --namespace=argo-events --from-file="./token" --from-file="./secret"
```

The Source is just a event trigger and in this case it will create
k8s pod and print body from PR event hook (from issue_comment event).

Note: the port forward of the pod with port 12000 is necesarry since we are using
minikube without any ingress on localhost!
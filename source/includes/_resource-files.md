# Resource Files

## What's a Resource file?

If you want a _good_ explaination of what i'm about to gloss over, [go here.](https://kubectl.docs.kubernetes.io/pages/kubectl_book/resources_and_controllers.html)

Resource files are `yml` files that describe a **resource**. For your app, you'll be creating **two** resource files:

- one for your **app**
- one for your **virtual service**

## Your app's resource file

```yaml
# Example app.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: api-test
  template:
    metadata:
      labels:
        app: api-test
    spec:
      containers:
        - name: api-test
          # Change to your image's url
          image: docker.io/kennethreitz/httpbin
          ports:
            - containerPort: 80
          resources:
            limits:
              memory: "50M"
              cpu: "25m"
---
# Explained to the left
apiVersion: v1
kind: Service
metadata:
  name: api-test
spec:
  selector:
    # Needs to match `labels.app` under the podSpec above
    app: api-test
  ports:
    - port: 443
      # Needs to match containerPort
      targetPort: 80
      name: http-api-test
```

The snippet to the right describes a Kubernetes [Deployment.](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

Again, if you want to know what each thing means, there's _plenty_ of much better explainations out there.

The things _you'll_ be changing here is wherever you see `api-test` and the `image` property under `containers`

1. Change occurences of `api-test` to **the name of your app** and the `image` value to a **url to your image**.
   - You can get the URL to your image from your project's Container Registry page.
2. If your app does not listen on port 80, change **containerPort** to whatever.

Down below the Deployment, you'll notice another kind of resource which is the Kubernetes **Service** that needs to accompany your Deployment.

<aside class="notice">
A Kubernetes Service is an abstraction layer which defines a logical set of
Pods and enables external traffic exposure,
load balancing and service discovery for those Pods.
</aside>
[Link to where I got that ☝️ quote.](https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/)

### Customize the Service

For the **Service**, customize the **targetPort** to whatever your **containerPort** is in the Deployment. And also change `api-test` to whatever you did in the Deployment.

## Your app's virtual service

```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: api-test
spec:
  hosts:
    - "api-test.staplcotn.us"
  gateways:
    - default/istio-custom-gateway
  http:
    - name: "api-test-api"
      route:
        - destination:
            host: api-test
```

Your app needs a [`virtualservice`](https://istio.io/latest/docs/reference/config/networking/virtual-service/) so Kubernetes will route requests to your Service. You'll note how the Service in the last section has _very_ little logic – because it all goes in here.

Essentially, a VirtualService **ties a Host Name to your app**.
You can configure routing rules in your VirtualService if you see fit. Consult the [documentation](https://istio.io/latest/docs/reference/config/networking/virtual-service/).

## Commiting your resource files

```shell
1. Clone
$ git clone https://gitlab.staplcotn.com/devops/aws

2. Add your new files...

3. Commit your new files and push to git
$ git commit -m "Add my App" && git push

```

You'll **commit your two new resource files to our devops/aws repo** so we can track their changes and apply them to the cluster with Argo.

Note: we haven't figured out how we want to organize the folder's inside the repo, so just throw your files in the `dev` folder there.

---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ

toc_footers:
  - <a href='https://staplcotn.okta.com/app/UserHome'>Staplcotn Employee Apps</a>
  - <a href='https://www.npmjs.com/package/@okta/okta-react'> @okta/okta-react SDK</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>
includes:
  - getting-started
  # - containers-&-u
  - install-private-packages
  - tag-your-container
  - resource-files
  - a_new_project
search: true

code_clipboard: true
---

<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">
<style>
  .mermaid {margin-left: 2rem;}
</style>

# Overview

## Intent of this Document

```text
  # Examples and code snippets will be over here.
```

This documentation is going to show you how to run your app on our Kubernetes cluster and get internet traffic to it.

To do so, you'll:

- Containerize your app into a [container](https://www.docker.com/resources/what-container).

- Use Gitlab Runner to build your container for you

- (and maybe) Use environment variables and utilize [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) to store API keys.

The other sections under "Getting Started" are not necessary reads, but I do try to give context about some **inner workings of our cluster that aren't easily googleable** and could be confusing later in the document and while troubleshooting.

## Networking in our cluster

Networking in our Kubernetes cluster revolves around [Istio](https://istio.io/latest/docs/concepts/what-is-istio/) and it's `virtualservices`. By configuring a VirtualService for your app ([see below](#resource-files)), you'll tell Kubernetes (and, more technically, Istio) what _hostname_ (such as `https://api-test.staplcotn.com`) your app expects to be registered on.

## Applying changes to our cluster

Common practice in K8Sland is to store as much of your cluster's configuration in a `git` repo and apply it with a tool like [Argo](https://argoproj.github.io/argo-cd/). In our case, our cluster's configuration is stored in [devops/aws](https://gitlab.staplcotn.com/devops/aws) and Argo is available at [https://argo.dev.staplcotn.com](https://argo.dev.staplcotn.com)

<div class="mermaid">
     graph TD
      style C fill:#FFAAAA;
      style apply fill:#D4EE9F;
      A[Tell Argo to update Application] --> B[Argo applies .yml file to cluster for you]
      B-->C[fa:fa-ban Error fetching image]
      B-->apply(fa:fa-check);

</div>

## To Document:

- K8s & docker pull permissions

- Argo weirdness (paths)
- [configmaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#define-a-container-environment-variable-with-data-from-a-single-configmap)

- NPM token in envorinment so builds work
<script>mermaid.initialize({startOnLoad:true});</script>

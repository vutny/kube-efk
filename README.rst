=========================================
Centralized logging system for Kubernetes
=========================================

All-in-one Helm chart for setting up a collection of all logs from Kubernetes
cluster using EFK stack (ElasticSearch, Fluent Bit and Kibana).

Requirements
============

* Kubernetes, up and running, at least v1.10
* Helm, and Tiller deployed in the cluster

Setup
=====

1. Install EFK stack into Kubernetes:

   .. code:: shell

       helm repo add kube-efk \
           https://raw.githubusercontent.com/vutny/kube-efk/master

       helm install --name efk --namespace logging kube-efk/kube-efk

   You should see cluster resources being created.

2. Access Kibana via port forwarding:

   .. code:: shell

       POD=$(kubectl get pod -l app=kibana -n logging -o jsonpath="{.items[0].metadata.name}")
       kubectl port-forward "$POD" 5601 -n logging

3. Open http://localhost:5601 and discover ``kubernetes_cluster-*`` index.

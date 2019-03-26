=========================================
Centralized logging system for Kubernetes
=========================================

Very brief HOWTO for collecting all logs from your Kubernetes cluster using
EFK stack (ElasticSearch, Fluent Bit and Kibana).

Setup
=====

1. Install ElasticSearch Operator:

   .. code:: shell

       helm repo add akomljen-charts \
           https://raw.githubusercontent.com/komljen/helm-charts/master/charts/

       helm install --name es-operator \
           --namespace logging \
           akomljen-charts/elasticsearch-operator

2. Verify that Custom Resource Definition has been created:

   .. code:: shell

       kubectl get CustomResourceDefinition

3. Install EFK stack:

   .. code:: shell

    helm install --name efk \
        --namespace logging \
        --set fluent-bit.backend.es.time_key='@ts' \
        akomljen-charts/efk

4. Verify that pods and the cronjob are in place:

   .. code:: shell

       kubectl get pods -n logging
       kubectl get cronjob -n logging

5. Access Kibana via port forwarding:

   .. code:: shell

       POD=$(kubectl get pod -l app=kibana -n logging -o jsonpath="{.items[0].metadata.name}")
       kubectl port-forward "$POD" 5601 -n logging

6. Open http://localhost:5601 and add ``kubernetes_cluster-*`` index.

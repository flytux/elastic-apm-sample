### install elasticsearch
helm upgrade -i elasticsearch -f elastic-values.yaml -n elk-stack charts/elasticsearch-8.5.1.tgz --create-namespace

### install kibana
helm upgrade -i kibana -f kibana-values.yaml charts/kibana-8.5.1.tgz -n elk-stack

### elastic user password check
kubectl get secrets --namespace=elk-stack elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d

### install apm-server
helm upgrade -i apm-server -f apm-server-values.yaml charts/apm-server-8.5.1.tgz -n elk-stack

### install kube-state-metrics
kubectl apply -k manifests/kube-state-metrics/

### install elastic-agent
kubectl apply -f manifests/elastic-agent-standalone-kubernetes.yaml

### install otel-collector
kubectl apply -k manifests/otel-collector/

### install  java sample
cd manifests/java-sample/sample-src/
docker build -t rolldice:v1 .

kubectl apply -k manifests/java-sample/

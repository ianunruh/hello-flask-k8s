# hello-flask-k8s

Run a Flask app on Kubernetes, monitored by Telegraf/InfluxDB/Grafana.

## Usage

```bash
git clone https://github.com/ianunruh/hello-flask-k8s.git
cd hello-flask-k8s
```

Start by deploying InfluxDB

```bash
kubectl create -f influxdb-service.yml
kubectl create -f influxdb-deployment.yml
```

Now create a database for our metrics to go into

```bash
POD_NAME=$(kubectl get pod -l app=influxdb -o "jsonpath={.items[0].metadata.name}")
kubectl exec $POD_NAME -it -- influx
create database production
exit
```

Then deploy Grafana

```bash
kubectl create -f grafana-service.yml
kubectl create -f grafana-deployment.yml
```

And finally, deploy the example Flask app.

```bash
kubectl create -f hello-flask-service.yml
kubectl create -f hello-flask-deployment.yml
```

Find the external IP addresses using `kubectl get svc`.

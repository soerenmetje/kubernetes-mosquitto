# Mosquitto MQTT Broker on Kubernetes

Deploy a Mosquitto MQTT broker on a Kubernetes cluster.
The deployment uses the [eclipse-mosquitto](https://hub.docker.com/_/eclipse-mosquitto) Docker image.

## Prerequisites
- A Kubernetes cluster is up and running
- A Cluster Issuer is up and running in the cluster
- A port is open in the firewall (this setup uses `31883`)
- A subdomain that points to the cluster

## Setup
1. Replace the default Mosquitto password in [configmap-psw.yaml](deploy/configmap-psw.yaml). To generate the hashed password, you can run:
```shell
docker run --rm -it eclipse-mosquitto:2.0 sh -c "mosquitto_passwd -c -b /pws.txt admin mystrongmqttpassword && cat pws.txt"
```
2. Check if Mosquitto config is ok in [configmap.yaml](deploy/configmap.yaml).
3. Replace `mqtt.example.com` with your subdomain in [ingress.yaml](deploy/ingress.yaml). 
4. Create the resources on your Kubernetes cluster:
```shell
kubectl create namespace mqtt
kubectl apply -n mqtt -f deploy/
```

## Test Deployment

After a successful deployment, the MQTT broker should be available on port `31883`. To verify that, you can use the Unix tool netcat:
```shell
netcat -vz 123.456.789.123 31883
```

For testing the MQTT broker, you may use the desktop application [MQTTX](https://mqttx.app/). 
It enables connecting to the broker, as well as, sending and receiving messages.

You should be able to connect to the broker on `mqtts://mqtt.example.com` using the specified user and password.

## Sources
- https://www.intuz.com/blog/how-to-deploy-mqtt-broker-in-a-kubernetes-cluster
- https://moreillon.medium.com/encrypted-mosquitto-mqtt-broker-in-kubernetes-26bb7acd11c7
- https://mosquitto.org/man/mosquitto_passwd-1.html
- https://hub.docker.com/_/eclipse-mosquitto
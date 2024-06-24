# Mosquitto MQTT Broker on Kubernetes

Deploy a Mosquitto MQTT broker on a Kubernetes cluster.
The deployment uses the [eclipse-mosquitto](https://hub.docker.com/_/eclipse-mosquitto) Docker image.

## Setup
1. Ensure, that the port `31883` is open in the firewall.
2. Replace the default Mosquitto password in [configmap-psw.yaml](deploy/configmap-psw.yaml). To generate the hashed password, you can run:
```shell
docker run --rm -it eclipse-mosquitto:2.0 sh -c "mosquitto_passwd -c -b /pws.txt admin mystrongmqttpassword && cat pws.txt"
```
3. Check if Mosquitto config is ok in [configmap.yaml](deploy/configmap.yaml).
4. Set up the deployment:
```shell
kubectl create namespace mqtt
kubectl apply -n mqtt -f deploy/
```

## Test Deployment

After a successful deployment, the MQTT broker should be available on port `31883`. To verify that, you can run:
```shell
netcat -vz 123.456.789.123 31883
```

For testing the MQTT broker, you may use the desktop application [MQTTX](https://mqttx.app/). 
It enables connecting to the broker, as well as, sending and receiving messages.


## Sources
- https://www.intuz.com/blog/how-to-deploy-mqtt-broker-in-a-kubernetes-cluster
- https://mosquitto.org/man/mosquitto_passwd-1.html
- https://hub.docker.com/_/eclipse-mosquitto
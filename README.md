
![image](https://user-images.githubusercontent.com/17733053/147707527-a57da654-bc27-4599-a888-6055087d4044.png)


# K8s deploy

## Flask-Mysql-Volume


### Build images

```bash
eval $(minikube docker-env)

docker pull mysql

docker build . -t flask-api
```

### Secrets

```bash
#Get the base64 version of the password
echo -n <PASSWORD> | base64
```

```bash
apiVersion: v1
kind: Secret
metadata:
  name: flaskapi-secrets
type: Opaque
data:
  db_root_password: <Insert your base64 password here>
```

```bash
kubectl apply -f secrets.yml
```

### Persistent Volume

```bash
kubectl apply -f persistent-volume.yml 
```

> To see if the details of your created resources via kubectl describe pv mysql-pv-volume and kubectl describe pvc mysql-pv-claim


### Mysql Server

```bash
#Run mysql deployment
kubectl apply -f mysql-deployment.yml
```

```bash
 kubectl exec -ti mysql-<random-digits-here> /bin/bash

 mysql -u root -p

 <Insert the password>

CREATE DATABASE flaskapi;

USE flaskapi;

CREATE TABLE users(user_id INT PRIMARY KEY AUTO_INCREMENT, user_name VARCHAR(255), user_email VARCHAR(255), user_password VARCHAR(255));

quit;

exit
```

### Flask API

```bash
kubectl apply -f flaskapp-deployment.yml

## Get the address of the service
minikube service flask-service
```








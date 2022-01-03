
![image](https://user-images.githubusercontent.com/17733053/147707527-a57da654-bc27-4599-a888-6055087d4044.png)


# K8s deploy

## Flask-Mysql-Volume


### Build images

- Let's build the mysql database images and flask API first

```bash
eval $(minikube docker-env)

docker pull mysql

docker build . -t flask-api
```

### Secrets

- Secret kubernets allow you to store sensitive information
- Let's choose a password for our database and save it
> See more [here](https://kubernetes.io/docs/concepts/configuration/secret/)


- Choose a password for the database and replace it in the command below
```bash
#Get the base64 version of the password
echo -n <PASSWORD> | base64
```

- Go to the ```secrets.yml``` file and add the base64 password in the ```db_root_password``` field as shown below

```bash
apiVersion: v1
kind: Secret
metadata:
  name: flaskapi-secrets
type: Opaque
data:
  db_root_password: <Insert your base64 password here>
```

- Run the command to apply secrets

```bash
kubectl apply -f secrets.yml
```

### Persistent Volume

- We will need to create a volume to persist the data to be stored in our database

- If the pod is destroyed all information will still be kept

- In this example we will create a volume with 2gb

- Run the command below to apply the volume

```bash
kubectl apply -f persistent-volume.yml 
```

### Mysql Server

- Since we already built the image in the first step, let's just run the command below to create our pod with mysql
- We also created a service so that we can access our database through it. If the database pod becomes unavailable and another one is created, our application is not dependent

```bash
#Run mysql deployment
kubectl apply -f mysql-deployment.yml
```
- Let's make a connection to the database to create the tables that will be used by the API
- Since we are going to do this configuration outside of kubernetes, we will use the password chosen in the first steps without being converted to base64

```bash
#Get the pods
kubectl get pods
 
NAME                    READY   STATUS    RESTARTS   AGE
mysql-99fb77bf4-8sg6w   1/1     Running   0          17s

#Replace the <randim-digits-here> in the below command for the values from the mysql pod found the previus command
kubectl exec -ti mysql-<random-digits-here> /bin/bash

#Run the command to enter with root user
mysql -u root -p
 
#Insert the password before convert to base64
<Insert the password>
 
#Create the database 
CREATE DATABASE flaskapi;

#Select the database
USE flaskapi;

#Create the table
CREATE TABLE users(user_id INT PRIMARY KEY AUTO_INCREMENT, user_name VARCHAR(255), user_email VARCHAR(255), user_password VARCHAR(255));

quit;

exit
```

### Flask API

- Almost there, now we have to go up our Flask API
- We will have 3 replicas for our API
- Run the command below to apply the API

```bash
kubectl apply -f flaskapp-deployment.yml
```

- We get the public url for our API

```bash
## Get the address of the service
minikube service flask-service
```


![image](https://user-images.githubusercontent.com/17733053/147707527-a57da654-bc27-4599-a888-6055087d4044.png)


# K8s deploy

## Wordpress Deploy

- Let's execute the application with the following command

```bash
#Create pod
kubectl apply -f wp-dumb.yaml
```
- Get the status 

```bash
#See result
kubectl get pods

#See pod by label
kubectl get pods -l app=wordpress
```




![image](https://user-images.githubusercontent.com/17733053/147707527-a57da654-bc27-4599-a888-6055087d4044.png)


# K8s deploy

## Wordpress Deploy

```bash
#Create pod
kubectl apply -f wp-dumb.yaml

#See result
kubectl get pods

#See pod by label
kubectl get pods -l app=wordpress

#Create a file
echo "lala" > file.txt

# copiando um arquivo para o container
$ kubectl cp file.txt meu-blog:/var/www/html

```


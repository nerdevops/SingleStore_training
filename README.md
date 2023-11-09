# SingleStore_training
Basic SingleStore Learning Project

 **SingleStore**
     ![Logo](/img/SS.png)


Requirements:
 - Docker Desktop

1. Create Container for the traning
```console
docker run -d -p 22:22 -p 3306:3306 -p 8080:8080 --name master singlestoretraining/ubuntu_master:latest
docker run -d --name child singlestoretraining/ubuntu_child:latest 
docker run -d --name leaf1 singlestoretraining/ubuntu_leaf1:latest 
docker run -d --name leaf2 singlestoretraining/ubuntu_leaf2:latest
```
>> The containers should be up like the example bellow
```console
docker ps -a --format 'table {{.Names}}\t{{.Status}}'
leaf2                Up 9 minutes    singlestoretraining/ubuntu_leaf2:latest
leaf1                Up 9 minutes    singlestoretraining/ubuntu_leaf1:latest
child                Up 10 minutes   singlestoretraining/ubuntu_child:latest
master               Up 10 minutes   singlestoretraining/ubuntu_master:latest
```

2. Verify the ipadd of the containers
```console
docker network inspect bridge | grep 'Name\|IPv4Address'
        "Name": "bridge",
                "Name": "master",
                "IPv4Address": "172.17.0.2/16",
                "Name": "child",
                "IPv4Address": "172.17.0.3/16",
                "Name": "leaf2",
                "IPv4Address": "172.17.0.5/16",
                "Name": "leaf1",
                "IPv4Address": "172.17.0.4/16",
```
3. Configure the ssh keys, config permitions
```console
cp ~/Downloads/singlestorekey.pem ~/.ssh/singlestorekey.pem
chmod 600 ~/.ssh/singlestorekey.pem
ssh -i ~/.ssh/singlestorekey.pem ubuntu@localhost
```
# SingleStore_training
Basic SingleStore Learning Project

 **SingleStore**
     ![Logo](/img/SS.png)


Requirements:
 - Docker Desktop

1. Create Container for the traning
```console
docker run -d -p 22:22 -p 3306:3306 -p 8080:8080 -p 8081:8081 --name master singlestoretraining/ubuntu_master:latest
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
4. On Master Install SingleStore tools
```console
ssh -i ~/.ssh/singlestorekey.pem ubuntu@localhost
sudo apt update -y
apt-cache policy apt-transport-https
wget -O - 'https://release.memsql.com/release-aug2018.gpg' 2>/dev/null | sudo apt-key add - && apt-key list
echo "deb [arch=amd64] https://release.memsql.com/production/debian memsql main" | sudo tee /etc/apt/sources.list.d/memsql.list
sudo apt update && sudo apt -y install singlestoredb-studio singlestoredb-client singlestoredb-toolbox
sudo apt -y install singlestoredb-studio singlestore-client singlestoredb-toolbox
```

### How to perform live deployment via browser of a SingleStore DB Cluster
5. Verify if all tools are installed
```console
ubuntu@master:~$ apt -qq list singlestoredb-toolbox
singlestoredb-toolbox/unknown,now 1.17.5 amd64 [installed]
ubuntu@master:~$ apt -qq list singlestore-client
singlestore-client/unknown,now 1.0.7 amd64 [installed]
ubuntu@master:~$ apt -qq list singlestoredb-studio
singlestoredb-studio/unknown,now 4.0.15 amd64 [installed]
```
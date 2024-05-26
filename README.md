## Create a next App
## Create an AWS EC2 server
-Update server, install docker and run docker

```sudo apt-get update && sudo apt-get install docker.io -y && sudo systemctl start docker```

-Change permissions

```sudo chmod 666 /var/run/docker.sock```

-Enable docker

```sudo systemctl enable docker```

-Test docker

```docker ps```

### Create a Runner inside EC2
-Go to Settings -> Code and automations -> Actions -> Runners

-Create a new self-hosted runner (name of runner: ec2)

-Install svc.sh

```sudo ./svc.sh install```

-Start the service

```sudo ./svc.sh start```

## Create a .github/workflows/cicd.yml

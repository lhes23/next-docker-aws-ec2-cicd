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

### Open Security group
-Go to Security Groups and open port 3000


## Create a .github/workflows/cicd.yml

```name: Deploy Next JS Application
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Source
        uses: actions/checkout@v4
      - name: Create .env file
        run: echo "MONGO_URL = ${{ secrets.MONGO_URL }}" >> .env
      - name: Build docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/next-docker-aws-ec2-cicd .
      - name: Login to Dockerhub
        run: docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
      - name: Publish image to Dockerhub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/next-docker-aws-ec2-cicd

  deploy:
    needs: build
    runs-on: self-hosted
    steps:
      - name: Pull image from Dockerhub
        run: docker pull ${{ secrets.DOCKER_USERNAME }}/next-docker-aws-ec2-cicd
      - name: Delete the old container
        run: docker rm -f nextjs-app-container
      - name: Run docker container
        run: docker run -d -p 3000:3000 --name nextjs-app-container ${{ secrets.DOCKER_USERNAME }}/next-docker-aws-ec2-cicd
```

### Create Secrets

-Go to Settings -> Security -> Secrets and Variables -> Actions

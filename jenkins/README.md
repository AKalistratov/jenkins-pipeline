```
docker build --tag jenkins-in-docker-with-docker .
docker run -dp 8080:8080 -p 50000:50000 -v C:\Users\Ilya\jenkins_home:/var/jenkins_home --group-add 0 -v "//var/run/docker.sock:/var/run/docker.sock" jenkins-in-docker-with-docker
```
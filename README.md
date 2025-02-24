# k-sharing

## ppt prompt
```
write a ppt for my internal tech knowledge sharing, introduce a simple fullly dockerized devop system that use jenkins, registry:2, containrrr/watchtower
- use registry:2 as a lan image registry, speed up docker push and pull, and notify watchtower to check updates
- use jenkins to pull code and build and push docker images to local registry
- use jenkins docker agents so that we can use containers to build code of various languages, e.g. mvn to build java backend and nodejs to build vuejs frontend.
- use containrrr/watchtower to update containers, will be triggerd by jenkins using http api
- explain why use dockerized containers
```

https://g.co/gemini/share/667d9847a3f5

## componets

### local registry

```sh
# tag
docker pull hello-world
docker tag hello-world registry.fit2cloud.org/hello-world

# push
docker push  registry.fit2cloud.org/hello-world
```

### jenkins

```conf
pipeline {
    agent { docker { image 'maven:3.8.6-jdk-11' } }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    def imageName = "your-lan-ip:5000/your-app:${BUILD_NUMBER}"
                    sh "docker build -t ${imageName} ."
                    sh "docker push ${imageName}"
                    sh "curl -X POST http://watchtower:8080/v1/update?cleanup=true"
                }
            }
        }
    }
}
```

### trigger watchtower

```sh
curl -H "Authorization: Bearer Calong@2015" watchtower:8080/v1/update -v
```

### demo app

```sh
docker build -t registry.fit2cloud.org/demo-app:latest .
docker build --build-arg CACHEBUST=$(date +%s) -t registry.fit2cloud.org/demo-app:latest .
docker push registry.fit2cloud.org/demo-app:latest
```
# Example of build using pipeline and rancher-compose

```
withEnv(['RANCHER_COMPOSE_HOME=/usr/lib/rancher-compose', 'RANCHER_URL=http://rancher-master:8080/','RANCHER_SECRET_KEY=ABCD','RANCHER_ACCESS_KEY=ABCD']) {
    node {
       stage 'Prepare deploy'
       git credentialsId: 'projectname', url: 'git@your-git-repo'
       stage 'Deploy to UAT'
       sh '${RANCHER_COMPOSE_HOME}/deploy-stack.sh your-repo-git/deploy'
    }
    stage 'Validation'
    try {
        input message: 'UAT valid ?', ok: 'Yes please deploy to prod !'
    } catch (InterruptedException e) {
        node {
            sh '${RANCHER_COMPOSE_HOME}/rollback-stack.sh your-repo-git/deploy'
        }
        throw e
    }
    node {
       stage 'Deploy to Prod'
       sh '${RANCHER_COMPOSE_HOME}/confirm-deploy-stack.sh your-repo-git/deploy'
    }
    stage 'Validation in production'
    input message: 'Production OK ?', ok: 'Yes'

}
```

# Example of build using docker

```
node {
   stage 'Prepare docker image'
   sh 'echo "FROM debian:jessie" > Dockerfile'
   stage 'Build docker image'
   docker.build 'myimage'
}
```
See [jenkins pipeline documentation](https://go.cloudbees.com/docs/cloudbees-documentation/cje-user-guide/chapter-docker-workflow.html) to get more details.
 
 # Advanced example
 If you want to control docker version you can setup available versions and installation scripts in jenkins and then refer to it with tool keyword.
 ```
 node {
    stage 'Prepare docker image'
    sh 'echo "FROM debian:jessie" > Dockerfile'
    stage 'Build docker image'
    dockerHome = tool name: '1.10.3', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
    sh "${dockerHome}/bin/docker build ."
 ```
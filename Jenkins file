node{
   
   stage("App Build started"){
      echo 'App build started..'
      git credentialsId: '14c388c5-508a-4c98-9f36-f51da63c2784', url: 'https://github.com/itrainspartans/python-docker-app-openshift.git'
      }
   
   stage('Docker Build') {
     def app = docker.build "madhupk/python-newrelic"
    }
   
   stage("Tag & Push image"){
      withDockerRegistry([credentialsId: 'dockerID',url: ""]) {
          sh 'docker tag madhupk/python-newrelic madhupk/python-newrelic:dev'
          sh 'docker push madhupk/python-newrelic:dev'
          sh 'docker push madhupk/python-newrelic:latest'
      }
    }
   
   stage("App deployment started"){
     sh 'oc login --token=RRBjJ0hQ656AT-v9dtzA9uZ4cnRbwz4fba0fwm5KmFY --server=https://api.us-west-1.starter.openshift-online.com:6443'
     sh 'oc project itrainspartans'
     sh 'oc new-app --name python-newrelic madhupk/python-newrelic'
      sh 'oc rollout latest dc/python-newrelic -o json' 
      sh 'oc rollout latest madhupk/python-newrelic --name python \
          --env NEWRELIC_LICENSE=97dddff513f56c90ce7a8fb577a6fca70649a81e \
                NEWRELIC_APPNAME=python-newrelic'
     //sh 'oc expose svc python-newrelic' 
    }
   
    stage('App deployed to Openshift environment') {
     echo 'App deployed to Openshift environment..'
    }

  }

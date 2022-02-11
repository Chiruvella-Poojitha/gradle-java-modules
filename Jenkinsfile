pipeline {
agent {
        kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: dc-pipeline
spec:
  securityContext:
    runAsUser: 10000
    runAsGroup: 10000
  containers:
  - name: jnlp
    image: 'jenkins/jnlp-slave:4.3-4-alpine'
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
  - name: gradle     # Jenkins agent container to run maven commands
    image: node:14
    command:
    - cat
    tty: true
    imagePullPolicy: Always
    securityContext: # https://github.com/GoogleContainerTools/kaniko/issues/681
      runAsUser: 0
      runAsGroup: 0

"""
        }
    }
  
  stages {
        stage ('Build: gradle') {
            steps {
                container('gradle') {
                    sh '''
		           gradle init --type pom
                            gradle build
		    '''
                }
            }
        }


        



    }
}

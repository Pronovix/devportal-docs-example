#!groovy

java.lang.String curl = "curl --resolve mark.site.devportal.io:80:127.0.0.1 -X POST"

pipeline {
	agent any

    stages {
        stage('Check') {
            when { branch 'master' }
            steps {
                sh 'docker run --rm -v `pwd`:/srv/tests testthedocs/vale-demo:0.0.7'
            }
        }
        stage('Upload') {
            when { branch 'master' }
            steps {
                withCredentials([usernamePassword(credentialsId: 'devportal', usernameVariable: 'BASE_URL', passwordVariable: 'TOKEN')]) {
                    sh """
                        for i in `find * -type d`; do 
                            for j in `find \$i -name '*.yaml'`; do 
                                $curl -H "X-Project-Id: \$i" -H "Content-Type: text/yaml" --data-binary @\$j ${BASE_URL}/dp-trigger/${TOKEN}
                            done
                            for j in `find \$i -name '*.md'`; do 
                                $curl -H "X-Project-Id: \$i" --data-binary @\$j ${BASE_URL}/dp-trigger/${TOKEN}/field_`basename \$j .md`
                            done
                        done
                    """
                }
            }
        }
    }
}

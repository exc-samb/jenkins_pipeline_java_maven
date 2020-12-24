pipeline {
    environment {
        projectName = 'test-multibranch'                 //Nombre del proyecto que se mostrará en SonarQube
        projectKey= 'multibranch'                    //Clave del proyecto que se mostrará en SonarQube
        sources='.'                                 //Ruta de los archivos fuentes que se deben analizar
        scanner = "sonar-scanner-4.2.0.1873"        //Nombre del scanner que analizará los archivos. Pueden ver los disponibles en: 
                                                    //Administrar Jenkins > Global Tool Configuration > SonarQube Scanner for MSBuild > Instalaciones de SonarQube scanner for MSBuild
    }
    agent {
        label 'master'
    }
    stages {
        stage('Checkout'){
            git url: 'https://github.com/TTFHW/jenkins_pipeline_java_maven.git'
            def mvnHome = tool 'Maven 3.6.3'
        }
        stage('Build'){
            bat "${M2_HOME}/bin/mvn clean package"
            step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        }
        stage('Code Analysis') {
            steps {
                echo 'Code analysis with SonarQube started'
                withSonarQubeEnv('SonarQube-local') {
                    script {
                        bat "${scanner}/bin/sonar-scanner -Dsonar.projectKey=${projectKey} -Dsonar.projectName=${projectName} -Dsonar.sources=${sources} -Dsonar.version=${env.BUILD_NUMBER}"
                    }
                }
                echo 'Code analysis with SonarQube finished'
            }
        }
    }
    post {
        always{
            echo 'Finished. Clean Up our workspace'
            deleteDir()
        }
    }
}

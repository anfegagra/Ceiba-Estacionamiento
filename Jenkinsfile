pipeline {
	agent {
		label 'Slave_Induccion'
	
	}
	
	options {
		buildDiscarder(logRotator(numToKeepStr: '3'))
		disableConcurrentBuilds()
	
	}
	
	tools {
		jdk 'JDK8_Centos'
		gradle 'Gradle4.5_Centos'
	
	}
	
	stages {
		stage('Checkout'){
			steps{
				echo "------------>Checkout<------------"
				checkout([$class: 'GitSCM', branches: [[name: '*/master']],
				doGenerateSubmoduleConfigurations: false, extensions: [], gitTool:
				'Git_Centos', submoduleCfg: [], userRemoteConfigs: [[credentialsId:
				'GitHub_anfegagra', url:
				'https://github.com/anfegagra/Ceiba-Estacionamiento.git']]])
			}
		}
		
		stage('Unit Tests') {
			steps {
				echo "------------>Unit Tests<------------"
				sh 'gradle --b ./build.gradle test'

			}
		
		}
		
		stage('Integration Tests') {
			steps {
				echo "INTEGRATION TESTS"
			
			}
		}
		
		stage('Static Code Analysis') {
			steps{
				echo '------------>Análisis de código estático<------------'
				withSonarQubeEnv('Sonar') {
					sh "${tool name: 'SonarScanner',
					type:'hudson.plugins.sonar.SonarRunnerInstallation'}/bin/sonar-scanner"
				}
			}
		}
		
		stage('Build') {
			steps{
				echo "------------>Build<------------"
				//Construir sin tarea test que se ejecutó previamente
				sh 'gradle --b ./build.gradle build -x test'
			}
		}
	
	
	}
	
	post {
		always {
			echo "This will always run"
		
		}
		
		success {
			echo 'This will run only if successful'
			junit '**/build/test-results/test/*.xml'
		
		}
		
		failure {
			echo 'This will run only if failed'
			mail (to: 'andres.granados@ceiba.com.co',subject: "Failed Pipeline:${currentBuild.fullDisplayName}",body: "Something is wrong with ${env.BUILD_URL}")

		}
		
		unstable {
			echo "run unstable"
		
		}
		
		changed {
			echo "Pipeline state has changed"
		
		}

		
	
	}


}
// Set your project Prefix using your GUID
def prefix      = "${env.GUID}"

// Set variable globally to be available in all stages
// Set Maven command to always include Nexus Settings
def mvnCmd      = "mvn -s ./nexus_openshift_settings.xml"
// Set Development and Production Project Names
def devProject  = "${prefix}-tasks-dev"
def prodProject = "${prefix}-tasks-prod"
// Set the tag for the development image: version + build number
def devTag      = "0.0-0"
// Set the tag for the production image: version
def prodTag     = "0.0"
def destApp     = "tasks-green"
def activeApp   = ""


pipeline {
    agent {
	kubernetes {
	    label "maven-appdev"
	    cloud "openshift"
	    inheritFrom "maven"
	    containerTemplate {
		name "jnlp"
		image "image-registry.openshift-image-registry.svc:5000/${GUID}-jenkins/jenkins-agent-appdev:latest"
		resourceRequestMemory "1Gi"
		resourceLimitMemory "2Gi"
		resourceRequestCpu "500m"
		resourceLimitCpu "1"
	    }
	}
    }
    stages {
	stage('Checkout Source') {
	    steps {
		checkout scm

		script {
		    def pom = readMavenPom file: 'pom.xml'
		    def version = pom.version

		    // TBD: Set the tag for the development image: version + build number.
		    devTag = "${version}-" + currentBuild.number

		    // TBD: Set the tag for the production image: version
		    // Example: def prodTag = "0.0"
		    prodTag = "${version}"

		}
	    }
	}
    }
}


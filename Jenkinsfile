//START-OF-SCRIPT
node {
    def server = Artifactory.server 'artifactory'
    
    withCredentials([usernamePassword(credentialsId: 'artifactory',
                     usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        server.username = "${USERNAME}"
        server.password = "${PASSWORD}"
    }    
    
    def rtGradle = Artifactory.newGradleBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/jeremycook123/devops-webapp3.git'
    }

    stage ('Artifactory Configuration') {
        rtGradle.tool = "gradle-7.3" // Tool name from Jenkins configuration
        rtGradle.deployer repo: 'cloudacademy-gradle-repo', server: server
        rtGradle.resolver repo: 'jcenter', server: server
    }

    stage ('Gradle Build') {
        buildInfo = rtGradle.run rootDir: "./", buildFile: 'build.gradle', tasks: 'clean build'
    }

    stage ('Gradle Publish') {
        buildInfo = rtGradle.run rootDir: "./", buildFile: 'build.gradle', tasks: 'artifactoryPublish'
    }

    stage ('Artifactory Publish Build Info') {
        server.publishBuildInfo buildInfo
    }
}
//END-OF-SCRIPT
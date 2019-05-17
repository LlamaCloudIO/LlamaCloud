pipeline {
  agent any
  tools {
    jdk 'java11'
    gradle 'gradle'
  }
  options {
    buildDiscarder logRotator(numToKeepStr: '10')
  }
  environment {
    JAVA_OPTS = "-XX:MaxMetaspaceSize=1G -Dorg.gradle.daemon=false"
  }
  stages {
    stage('Clean') {
      steps {
        sh 'sh gradlew clean'
      }
    }
    stage('Test') {
      steps {
        sh 'sh gradle test'
        sh 'sh gradle detekt'
        junit testResults: '**/build/test-results/test/*.xml', allowEmptyResults: true
        checkstyle canRunOnFailed: true, pattern: "**/build/reports/checkstyle/checkstyle.xml, **/build/reports/detekt/detekt.xml"
      }
    }
    stage('Build') {
      steps {
        sh 'sh gradle assembleDist'
      }
    }
    /*stage('Documentation') {
      tools {
        jdk 'Java8'
      }
      steps {
        sh 'sh gradle dokkaJavadoc'
      }
    }*/
    stage('Archive') {
      steps {
        archiveArtifacts artifacts: '**/build/libs/*.jar'
        //zip archive: true, dir: 'build/javadoc/', glob: '', zipFile: 'cloud-javadocs.jar'
      }
    }
  }
}

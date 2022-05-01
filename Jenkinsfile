#!/usr/bin/env groovy
def gv
pipeline {
    agent none

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "mymaven"
        jdk "myjava"
    }

    stages {
        stage('Compile') {
            agent any
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/srinivoleti/DevOpsClassCodes.git'

                // Run Maven on a Unix agent.
                sh "mvn compile"
                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
            }
        stage('CodeReview'){
            agent any
            steps {
                sh "mvn pmd:pmd"
            }
            }
         stage('UnitTest'){
            agent any
            steps {
                sh "mvn test"
            }
            post{
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
         stage('MetricCheck'){
            agent any
            steps {
                sh "mvn cobertura:cobertura -Dcobertura.report.format=xml"
            }
            post{
                always{
                    cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
                }
            }
        }
         stage('Package'){
            agent {label 'Ubuntu_Slave'}
            steps {
                git 'https://github.com/srinivoleti/DevOpsClassCodes.git'
                sh "mvn package"
            }
        }
        }
    }

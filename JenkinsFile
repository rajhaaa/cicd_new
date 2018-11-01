#!groovy


pipeline {
    agent any
    tools {
        maven 'maven'
        nodejs 'NodeJS'
    }
    stages {
        stage('Clean') {
            steps {
                dir('edge') {
                    sh "mvn clean"
                }
            }
        }
        
        stage('Pre-Deployment Configuration ') {
            steps {
                dir('edge') {

                    println "Predeployment of Caches "
                    withCredentials([usernamePassword(credentialsId: "edge-ms-local-cred",
                            passwordVariable: 'apigee_pwd',
                            usernameVariable: 'apigee_user')]) {
                        script {
                            if (fileExists("resources/edge/env/${params.apigee_env}/caches.json")) {
                                sh "mvn apigee-config:caches " +
                                        "    -Papigee -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                        "    -Dusername=${apigee_user} " +
                                        "    -Dpassword=${apigee_pwd}"
                            }


                        if (fileExists("resources/edge/env/${params.apigee_env}/kvms.json")) {
                            sh "mvn apigee-config:keyvaluemaps " +
                                    "    -Papigee -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                    "    -Dusername=${apigee_user} " +
                                    "    -Dpassword=${apigee_pwd}"
                        }

                        if (fileExists("resources/edge/env/${params.apigee_env}/targetServers.json")) {
                            sh "mvn apigee-config:targetservers " +
                                    "    -Papigee -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                    "    -Dusername=${apigee_user} " +
                                    "    -Dpassword=${apigee_pwd}"
                        }
                    }
                }
                }
            }
        }
            stage('Build proxy bundle') {
                steps {
                    dir('edge') {
                        sh "mvn package -Papigee -Denv=${params.apigee_env} -Dorg=${params.apigee_org}"

                    }
                }
            }

            stage('Deploy proxy bundle') {
                steps {
                    dir('edge') {
                        withCredentials([usernamePassword(credentialsId: "edge-ms-${params.apigee_org}-cred",
                                passwordVariable: 'apigee_pwd',
                                usernameVariable: 'apigee_user')]) {
                            sh "mvn install -Papigee -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
                                    " -Dusername=${apigee_user} -Dpassword=${apigee_pwd}"
                        }
                    }
                }
            }
            stage('Post-Deployment Configurations for API ') {
                steps {
                    println "Skipped"
//                dir('edge') {
//                    println "Post-Deployment Configurations for API Products Configurations, App Developer and App Configuration "
//                    sh "mvn -Papigee -Denv=${params.apigee_env} -Dorg=${params.apigee_org} " +
//                            "    -Dapigee.config.options=create " +
//                            "    -Dusername=${params.apigee_user} -Dpassword=${params.apigee_pwd} " +
//                            "    apigee-config:apiproducts " +
//                            "    apigee-config:developers apigee-config:apps apigee-config:exportAppKeys"
//                }
                }
            }

            stage('Functional Test') {
                steps {
                    println "Skipped"
                }
            }

            stage('Coverage Test Report') {
                steps {

                    println "SKipped"
                }
            }

            stage('Functional Test Report') {
                steps {

                    println "Skipped"
                }
            }

            stage('Release to Artifactory') {
                steps {
                    println "Skipped"
                   // sh "mvn deploy"
                }
            }

        }
    }

  def appName='App_10'
    def snapName=''
    def deployName ='Dep_1'
    def exportFormat ='json'
    def configFilePath = "fileB"
    def fileNamePrefix ='exported_file_'
    def fullFileName="${appName}-${deployName}-${currentBuild.number}.${exportFormat}"
    def changeSetId=""
    def snapshotName=""
    def exporterName ='returnAllData' 

    //def namePath ="E2E/pipelineUpload/${currentBuild.number}"
    def namePath ='Comp_2'
    //def namePath ="Comp_2/${JOB_NAME}/${currentBuild.number}"
pipeline {
    agent any
    stages {
        
        stage('E2E_Pipeline1_1') {
            steps {
                echo "Running E2E_Pipeline1_1..........."
                echo "MY_PARAM=${env.MY_PARAM}"
            }
        }
        
        stage('Clone repository') {               
           steps{
                // checkout scm
                git branch: 'temp1', url: 'https://github.com/rajkumat01/samplejava10'
           }
        }     
        stage('Upload JSON'){
            steps{
                script{
                    sh "echo validating configuration file ${configFilePath}.${exportFormat}"
                    echo "name path ::::: ${namePath}"
                    changeSetId = snDevOpsConfigUpload(applicationName:"${appName}",target:'component',namePath:"${namePath}",configFile:"fileB.json",autoCommit:false,autoValidate:true,dataFormat:"${exportFormat}")
                    snDevOpsConfigUpload(applicationName:"${appName}",changesetNumber:"${changeSetId}",target:"component",dataFormat:"json",configFile:"FileC.json",namePath:"${namePath}",autoCommit:false,autoValidate:true)
                    snDevOpsConfigUpload(applicationName:"${appName}",changesetNumber:"${changeSetId}",target:"component",dataFormat:"json",configFile:"fileA.json",namePath:"${namePath}",autoCommit:true,autoValidate:true)
                    echo "validation result $changeSetId"
                }
            }
        }
        stage("Committing the changeset"){
            steps{
                script{
                    echo "Change set registration for ${changeSetId}"
                    changeSetRegResult = snDevOpsConfigRegisterPipeline(changesetNumber:"${changeSetId}")
                    echo "change set registration set result ${changeSetRegResult}"
                }
            }
        }
    }
 }

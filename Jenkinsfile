pipeline{
    agent any
    stages{
        stage('Stage 1'){
            when {
                buildingTag() && expression { sh([returnStdout: true, script: 'echo $TAG_NAME | tr -d \'\n\'']) }
            }
            steps{
                script{
                echo $TAG_NAME
                }
            }
        }
    }
}
pipeline{
    agent any
    stages{
        stage('Stage 1'){
            when {
                buildingTag()
            }
            steps{
                script{
                echo ${env.TAG_NAME}
                }
            }
        }
    }
}
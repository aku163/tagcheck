pipeline{
    agent any
    stages{
        stage('Stage 1'){
            when {
                buildingTag()
            }
            steps{
                script{
                sh """
                echo "${env.TAG_NAME}"
                """
                }
            }
        }
    }
}
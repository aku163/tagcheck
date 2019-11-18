pipelines{
    agent any
    stages{
        stage('Stage 1'){
            when {
                buildingTag()
            }
            steps{
                script{
                echo $TAG_NAME        
                }
            }
        }
    }
}
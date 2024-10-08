node {
    def app

    stage('Clone repository') {     
        checkout scm
    }

    stage('Update GIT') {
        script {
            // credentialsId는 앞선 단계에서 Jenkins에서 생성한 github credential 값으로 설정
            // withCredentials는 Jenkins Pipeline에서 사용되는 구문으로, credentials을 안전하게 처리하기 위한 방법 중 하나
            withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                
                // github email
                sh "git config user.email lilykim6353@gmail.com"
               
                // github user name
                sh "git config user.name soondool"

                sh "cat deployment.yaml"

                // sed 명령어를 이용하여 파일 내용 수정
                // deployment.yaml에서 image에 해당하는 '{aws ecr url}'을 찾은 후 '{aws ecr url}:buildimage job의 빌드 넘버'로 변경
                sh "sed -i 's+234311324580.dkr.ecr.ap-northeast-2.amazonaws.com/ecr.*+234311324580.dkr.ecr.ap-northeast-2.amazonaws.com/ecr:${DOCKERTAG}+g' deployment.yaml"
                
                sh "cat deployment.yaml"
                
                sh "git add ."
                
                // 변경 사항을  commit
                sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                
                // 변경 사항을 kubernetesmanifest repo에 push
                // kubernetesmanifest용 git repository 이름이 study-kubernetesmanifest_repo이 아닐 경우 하기 코드에서 수정
                sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/study-kubernetesmanifest_repo.git HEAD:main'
            }                
        }
    }
}

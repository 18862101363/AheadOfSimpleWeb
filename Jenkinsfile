// Jenkinsfile (Declarative Pipeline)
pipeline {
    //agent any // agent 指示 Jenkins 为整个 Pipeline 分配执行程序（在 Jenkins 环境中的任何可用代理/节点上）和工作空间

    parameters {
        choice choices: ['testing', 'production'], description: '选择测试（10.80.121.35）还是生产（10.80.121.36）环境进行发布', name: 'DEPLOY_ENV'
    }
    agent {
        label params.DEPLOY_ENV
        // 上面的 parameters ： DEPLOY_ENV 与我在jenkins 中添加的 slave node 指定的 label 对应的，这里就根据什么的选择在对应的主机运行 build
//        label 'testing'
    }
    

    stages {
        stage('Build') {
            input {     // 这个 directive 是 让用户确认的弹出框
                message 'should we continue?'
                ok 'OK'   //这个是 确认按钮的文字显示
                submitter 'admin'    // 指定必须是 admin 用户才可以提交
//                submitterParameter 'STAGE1_SUBMITTER'
            }

            steps {
                echo 'AheadOfSimpleWeb Building'
                sh 'mvn -v'
          
            }
        }
    }
    post {//下面是构建后执行的响应操作
        always {

            echo 'AheadOfSimpleWeb One way or another, I have finished'
        }   
        success {
            echo 'AheadOfSimpleWeb I succeeeded!'
        }
        unstable {
            echo 'AheadOfSimpleWeb I am unstable :/'
        }
        failure {
            echo 'AheadOfSimpleWeb I failed :('
        }
        changed {
            echo 'AheadOfSimpleWeb Things were different before...'
        }
    }


}

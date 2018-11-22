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
            //    build 'SimpleWeb'
            //    build job: 'SimpleWeb', propagate: false
             //     build job: 'SimpleWeb', parameters: [string(name: 'DEPLOY_ENV', value: 'testing')]
             //   build job: 'SimpleWeb', parameters: [string(name: 'DEPLOY_ENV', value: 'testing')], propagate: false, wait: false
                
//build step 可以指定构建其他的jenkins任务，如果指定的任务是参数化的，则需要配置相应的参数值。
// wait 选项是在触发构建另一个jenkins任务，当前构建是否需要等待那个任务执行完毕。如果是 true , 那当前构建就阻塞等待那个任务执行完，再执行当前任务的下面step或 stage，并且会返回那个任务构建的结果对象，通过该对象，可以获取到 类似于 currentBuild 对象的属性
//		wait如果设置为false，那么当前构建就不阻塞，触发构建另一个任务后，并继续执行自己的step或stage。另外，如果另一个任务构建stage块中有input块，那么要我们自己手动点击另一个任务，不然会一直阻塞
//propagate 选项，当wait设置为false，该 propagate配置失效。只有当wait设置为true, 该 propagate配置才生效。propagate： true 表示，当前构建阻塞等待另一个任务构建结束，如果另一个任务构建结果不为SUCCESS（ABORTED,FAILURE,unstable等）那么当前的构建也会被认为失败。propagate :false 表示，当前构建阻塞等待另一个任务构建结束，另一个任务构建结果不管如果，都不影响当前的构建结果。
//当 build step 写在 post命令块状态里，如 unstable,success,failure状态块中，propagate, wait 选项设置 false 则达到的效果等同于单独在downstream jenkins 任务中配置的 trigger upstream 
              //  script{
              //      def  returnVal =   build job: 'SimpleWeb', parameters: [string(name: 'DEPLOY_ENV', value: 'testing')], propagate: false
              //      echo "${returnVal.displayName}"   
              //      echo "${returnVal.result}"   
              //  }  
//trigger 指令块，和 build step 在jenkins 任务构建，upstream任务构建的console out 中看到当前任务触发downstream任务构建。而downstream任务构建console out开头则显示当前任务被哪一个upstream任务触发而构建，如：
// upstream AheadOfSimpleWeb:
//    Scheduling project: SimpleWeb
//    Starting building: SimpleWeb #91
//    或
//    Triggering a new build of SimpleWeb #91
// downstream SimpleWeb:
    //Started by upstream project "AheadOfSimpleWeb" build number 15   
                
                
                echo 'AheadOfSimpleWeb Building success'
            }
        }
    }
    post {//下面是构建后执行的响应操作
        always {

            echo 'AheadOfSimpleWeb One way or another, I have finished'
        }   
        success {
            echo 'AheadOfSimpleWeb I succeeeded!'
            build job: 'SimpleWeb', parameters: [string(name: 'DEPLOY_ENV', value: 'testing')], propagate: false, wait: false
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

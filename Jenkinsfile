//gitlab的凭证
def git_auth = "faaf8dd2-ff5c-4588-aca5-b0cd56df51de"
node {
  stage('拉取代码') {
    checkout([$class: 'GitSCM', branches: [[name: '*/master']],
    doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [],
    userRemoteConfigs: [[credentialsId: "${git_auth}", url:
    'git@github.com:endeavor66/tensquare_front.git']]])
  }
  stage('打包，部署网站') {
    //使用NodeJS的npm进行打包
    nodejs('nodejs12'){
      sh '''
      npm install
      npm run build
      '''
    }
    //=====以下为远程调用进行项目部署========
    sshPublisher(publishers: [sshPublisherDesc(configName: 'master_server',
      transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '',
      execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes:
      false, patternSeparator: '[, ]+', remoteDirectory: '/usr/share/nginx/html',
      remoteDirectorySDF: false, removePrefix: 'dist', sourceFiles: 'dist/**')],
      usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
  }
}

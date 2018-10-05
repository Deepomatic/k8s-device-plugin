node {
    checkout scm

    // For PRs Jenkins will give the source branch name
    if (env.CHANGE_BRANCH) {
        isPR = true
        // env.BRANCH_NAME=PR-1
        version = env.BRANCH_NAME
    } else {
        isPR = false
        // env.BRANCH_NAME=deepomatic/v1.10
        version = env.BRANCH_NAME.split('/')[1].substring(1)
    }

    def namespace = 'deepomatic'
    def repository = 'infinite-gpus-nvidia-k8s-device-plugin'

    def arch = 'amd64'

    ['ubuntu16.04', 'centos7'].each { os ->
        def noOSSuffix = (os == 'ubuntu16.04')

        def image = docker.build("${namespace}/${repository}:${version}-${env.BUILD_ID}-${os}",
                                 "-f docker/${os}/${arch}/Dockerfile .")

        if (! isPR) {
            echo "Mainline branch, pushing to repository"
            image.push("${version}-${os}")
            if (! noOSSuffix) {
                image.push("${version}")
            }
        }
    }
}

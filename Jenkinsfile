node {
    checkout scm

    // For PRs Jenkins will give the source branch name
    if (env.CHANGE_BRANCH) {
        isPR = true
        // env.BRANCH_NAME=PR-1
        version = env.BRANCH_NAME
    } else {
        isPR = false
        // env.BRANCH_NAME=v1.10
        version = env.BRANCH_NAME.substring('v'.size())
    }

    def namespace = 'deepomatic'
    def repository = 'shared-gpu-nvidia-k8s-device-plugin'

    def arch = 'amd64'

    def os_stages = [:]
    ['ubuntu16.04', 'centos7'].each { os ->
        os_stages["${os}"] = {
            stage("${os.capitalize()} variant") {
                def noOSSuffix = (os == 'ubuntu16.04')

                def image = docker.build("${namespace}/${repository}:${version}-${env.BUILD_ID}-${os}",
                                         "-f docker/${os}/${arch}/Dockerfile .")

                if (! isPR) {
                    echo "Mainline branch, pushing to repository"
                    image.push()
                    image.push("${version}-${os}")
                    if (! noOSSuffix) {
                        image.push("${version}")
                    }
                }
            }
        }
    }

    parallel os_stages
}

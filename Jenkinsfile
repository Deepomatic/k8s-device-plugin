node {
    checkout scm

    // For PRs Jenkins will give the source branch name
    if (env.CHANGE_BRANCH) {
        branch = env.CHANGE_BRANCH
        isPR = true
    } else {
        branch = env.BRANCH_NAME
        isPR = false
    }

    // env.JOB_NAME=infinite-gpus-nvidia-k8s-device-plugin/deepomatic/v1.10
    def args = env.JOB_NAME.split('/')
    def repository = args[0]
    def namespace = 'deepomatic'
    if (! isPR) {
        version = args[2].substring(1)
    }

    def arch = 'amd64'
    def os = 'ubuntu16.04'
    def noOSSuffix = true

    // TODO --cache-from latest
    def image = docker.build("${namespace}/${repository}:${env.BRANCH_NAME}-${env.BUILD_ID}-${os}",
                             "-f docker/${os}/${arch}/Dockerfile .")

    if (! isPR) {
        echo "Mainline branch, pushing to repository"
        image.push("${version}-${os}")
        if (! noOSSuffix) {
            image.push("${version}")
        }
    }
}

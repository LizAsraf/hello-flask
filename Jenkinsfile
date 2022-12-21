pipeline {
    agent builder
    tools {
        git 'Default'
    }
    triggers {
        gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    }

}
pipeline {
    agent any
    tools {
        git 'Default'
    }
    triggers {
        gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    }

}
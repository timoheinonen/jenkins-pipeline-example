stage ('checkout') {
    node {
        git 'https://github.com/zapppp/ui-unit-test-example'
        stash excludes: 'target/', name: 'sources'
    }
}
stage ('build') {
    node {
        unstash 'sources'
        withMaven(maven: 'M3', mavenLocalRepo: '', mavenSettingsConfig: '') {
            // Run the maven build
            sh "mvn compile"
        }
    }
}
    
stage ('analyze') {
    node {
        echo "Code Analysis"
    }
}

stage ('test') {
    node {
        wrap([$class: 'Xvfb']) {
            withMaven(maven: 'M3', mavenLocalRepo: '', mavenSettingsConfig: '') {
                // Run the maven build
                sh "mvn -B -Dmaven.test.failure.ignore verify"
            }
        }
        step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        step([$class: 'ArtifactArchiver', artifacts: '**/failed-gui-tests/*.png', fingerprint: true])
    }
    
    //def splits = splitTests count(2)
    //def branches = [:]
    //for (int i = 0; i < splits.size(); i++) {
        //def index = i // fresh variable per iteration; i will be mutated
        //branches["split${i}"] = {
            //node('remote') {
               // deleteDir()
            //    unstash 'sources'
             //   def exclusions = splits.get(index);
              //  writeFile file: 'exclusions.txt', text: exclusions.join("\n")
            //    sh "${tool 'M3'}/bin/mvn -B -Dmaven.test.failure.ignore test"
             //   junit 'target/surefire-reports/*.xml'
            //}
        //}
    //}
    //parallel branches
}

stage ('deploy') {
    node {
        echo "deploy"
    }    
}

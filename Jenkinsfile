stage('CI') {
    node {
        
		checkout scm
		//git branch: 'jenkins2-course', 
        //url: 'https://github.com/g0t4/solitaire-systemjs-course'
        
        //Install dependencies
        bat 'npm install'
        
        //Temporary Archiving-Stashing
        stash excludes: 'test-results/**', 
        includes: '**', 
        name: 'Temparchive'
        
        //Testing on phantom JS
        bat 'npm run test-single-run -- --browsers PhantomJS'
        
        // archive karma test results (karma is configured to export junit xml files)
        step([$class: 'JUnitResultArchiver', 
          testResults: 'test-results/**/test-results.xml'])
 
    }
}

node('panda') {
   bat 'dir'
    // on windows use: bat 'del /S /Q *'
    bat 'del /S /Q *'
    unstash 'Temparchive'
    // on windows use: bat 'dir'
    bat 'dir'
}
stage('ParallelTest') {
    
    parallel chrome: {
        runTests("Chrome")
        }, firefox: {
        runTests("Firefox")
}
}

def runTests(browser) {
    node {
        // on windows use: bat 'del /S /Q *'
        bat 'del /S /Q *'

        unstash 'Temparchive'

        // on windows use: bat "npm run test-single-run -- --browsers ${browser}"
        bat "npm run test-single-run -- --browsers ${browser}"
        step([$class: 'JUnitResultArchiver', 
              testResults: 'test-results/**/test-results.xml'])
    }
}

input 'Pipeline has paused and needs your input before proceeding Are u there...??'
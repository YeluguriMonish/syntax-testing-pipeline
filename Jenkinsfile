node {
    stage('Preparation') {

        def blueTargets = []
        def greenTargets = []
        String step = "blue"
        retry = true
        retryCount = 0

        targets.each { target ->
          if (target.deployTo == 'gaia') {
            blueTargets.add(target)
            greenTargets.add([target,[:]])
          }
        }

        while (retry) {
          if (!blueTargets.isEmpty()) {
            vars = [:]
            target = blueTargets.pop()
            step = "blue"
          } else if (!greenTargets.isEmpty()) {
            target = greenTargets.pop()
            step = "green"
          } else {
            break
          }

          try {
            if (step == 'blue') {
              // example of saving variable for second half of deployment vars['url'] = google.com
              greenTargets.each { greenTarget ->
                if (target.name == greenTarget[0].name) {
                  greenTarget[1].putAll(vars)
                }
              }
            } else if (step == 'green') {
              // vars = target[1] -> use vars to access values saved from first half
              // tar = target[0] -> access target values
            }
          } catch (Exception e) {
            echo "$e"

            // add user input to force exit or retry here 

            if (step == 'blue') {
              blueTargets.push(target)
            } else if (step == 'green') {
              blueTargets.push(target[0])
              greenTargets.push(target)
            }
            retryCount++
            if (retryCount > 3) {
              retry = false
              echo "retry count exceeded, exiting loop"
            }
          }
        }
    }
}

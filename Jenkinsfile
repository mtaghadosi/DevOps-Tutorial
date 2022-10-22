// positive messages begins with + and negetive ones begins with - 
// 
//
//
pipeline{
    agent any
    environment {
        pyInstallerOutput = ''                      //the variable that contains the PyInstaller results, this actually indicates that if PyInstaller is installed on system or not.
        NAME = 'Created by. Mohammad-Taghadosi'     //cr34tedby
    }
    stages{
        stage('pre-compile-stage'){
            steps{
                script{
                    echo '*Stage1 - Checking Pre requerments...*'
                    //check-to-see that python is installed or-not-m.taghadosi
                    echo '+ checking installation of python, Please wati...'
                    try
                    {
                        pyver=bat(returnStdout: true, script: '@python --version')
                        echo "+ The result of pycheck was: $pyver"
                        echo '+ Python is up and running on the system. '
                        echo '+ It\'s good to continue... '
                    }catch(Exception pynot){
                        //python not installed.
                        //here I am planning to install the Python in next version of pipeline. <m.Taghadosi>
                        echo '- Python not installed or version is under 3.'
                        echo '- Please install Python at least ver.3 and run the job again '
                        error '- The Exact error was: ' + pynot.toString()
                        //here also possible to install the python .... 
                    }
                    try
                    {
                        pipresult=bat(returnStdout: true, script: '@python -m pip --version')
                        echo "+ The result of pycheck was: $pipresult"
                        echo '+ Pip is up and running on the system. '
                        echo '+ It\'s good to continue... '
                    }catch(Exception _noPip){
                        echo '- Pip not installed. trying to install it...'
                        error '- The Exact error was: ' + _noPip.toString()
                        try
                        {
                            pipresult=bat(returnStdout: true, script: '@python3 -m pip install --user --upgrade pip')
                            echo '+ Pip installed. Continuing... '
                        }catch(Exception fatal){
                            echo '- Oh, something bad happened when trying to install the pip, check the log '
                            error ('- the error was: ' + fatal.toString())
                        }
                    }
                    try
                    {
                        venv=bat(returnStdout: true, script:'@python3 -m venv -h').trim()
                        echo "+ The result of venv-check was: $venv"
                        echo '+ venv is ready on the system'
                        echo '+ It\'s good to continue... '
                    }catch(Exception _notVenv){
                        echo '- venv not installed. trying to install it...'
                        error '- The Exact error was: ' + _notVenv.toString()
                        try
                        {
                            pipresult=bat(returnStdout: true, script: '@python3 -m pip install --user virtualenv')
                            echo '+ venv installed. Continuing... '
                        }catch(Exception fatl){
                            echo '- Oh, something bad happened when trying to install the venv, check the log '
                            error ('- The error was: ' + fatl.toString())
                        }
                    }
                }

            }
        }
        stage('compile-stage'){
            steps{
                script {
                        echo '*Stage2 - Trying to compile python program*'
                        output = bat (returnStdout: true, script: '@python ./codes/SayHello.py')
                        echo "+ The result of compile was: $output"
                        //checking application-output to see that 
                        //the app ran successfully <m.Taghadosi>
                        //or not
                        if ("$output".contains('Hello')) {
                            echo '+ Compile checked, no error found, Good to continue... '
                        }  else {
                            error('+ Error in python compilation, stopping NOW please check the log... ')
                        }
                    }
            }
        }
        stage('artifact-build-stage'){
            //in this stage i'm gonna compile the python app to an articat <m.Taghadosi>
            //and also copy that artifact to a repository like Nexus Or just a categorized folder.
            steps{
                //bat 'dir'
                //echo bat(returnStdout: true, script: 'set') //Environmental Variables - usefull sometimes <mt>
                script{
                    //first checking that do we have pyinstaller or not 
                    //if not the program tries to install it and if successfull <m.Taghadosi> 
                    //the artifact building starts if not pipeline stops
                    echo '*Stage3 - Trying to build python program and export the Artifact*'
                    try{
                        pyInstallerOutput = bat(returnStdout: true, script: '@python -m PyInstaller -v').trim()
                        //PyInstaller is up and running. good!
                        //let build-artifact
                        echo "+ PyInstaller Version: $pyInstallerOutput is up and running. good! building artifact"
                        directoryName = bat(returnStdout: true,script: '@echo %date:~-4,4%-%date:~-10,2%-%date:~7,2%-%time:~-11,2%%time:~-8,2%%time:~-5,2%').trim()
                        echo ("+ Current Artifact directory: $directoryName")
                        artifactResult = bat(returnStdout: true, script: "@python -m PyInstaller --specpath ./artifacts-repo/${directoryName}/spec --distpath ./artifacts-repo/${directoryName}/dist --workpath ./artifacts-repo/${directoryName}/build --onefile ./codes/SayHello.py")
                    }catch(Exception e){
                        echo '- PyInstaller verification failed! The Error was: ' + e.toString()
                        echo '+ Attemping to install pyInstaller'
                        //pyInstaller not present m.taghadosi
                        //trying to install it
                        try{
                            bat(returnStdout: true, script: '@pip install pyinstaller --no-warn-script-location')
                            echo '+ PyInstaller installed successfully, Now building Artifact.'
                            directoryName = bat(returnStdout: true,script: '@echo %date:~-4,4%-%date:~-10,2%-%date:~7,2%-%time:~-11,2%%time:~-8,2%%time:~-5,2%').trim()
                            echo ("+ Current Artifact directory: $directoryName")
                            artifactResult = bat(returnStdout: true,script: "@python -m PyInstaller --specpath ./artifacts-repo/${directoryName}/spec --distpath ./artifacts-repo/${directoryName}/dist --workpath ./artifacts-repo/${directoryName}/build --onefile ./codes/SayHello.py")
                            //if this block reaches to end it means successfull installation
                        }catch (Exception ee){
                            //error installing nothing more can be done. stoping pipeline.
                            echo('- Installation failed the error was: ' + ee.toString())
                            error('- PyInstaller installation failed, nothing more can be done by pipeline, try to see the logs, STOPING...')
                        }
                    }

                }
            }
        }
    }

    post{
        always{
            echo 'Attemping some cleanup works.'
            echo "${NAME}"
            //i want to add some cleanups later <m.Taghadosi>
        }
        success{
            echo 'The Pipeline execution was a SUCCESS.'
        }
        failure{
            echo 'Some stages failed, removing temp files.'
            //i want to add some cleanups later <m.Taghadosi>
        }
    }
}
// positive messages begins with + and negetive ones begins with - 
// 
//
//
pipeline{
    agent any
    environment {
        venv_name = ''                              //python virtual environment name.
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
                            error '- The error was: ' + fatl.toString()
                        }
                    }
                }

            }
        }
        stage('preparing-environment'){
            steps{
                script {
                        echo '*Stage2 - Preparing environment...*'
                        //venv name will be set acording to date and time of execution so this naming has two benefits:
                        //1. It's unique 
                        //2. It's meanfull.
                        //using the command i get that:
                        //echo %date:~-4,4%-%date:~-10,2%-%date:~7,2%-%time:~-11,2%%time:~-8,2%%time:~-5,2%

                        venv_name = bat(returnStdout: true,script: '@echo %date:~-4,4%-%date:~-10,2%-%date:~7,2%-%time:~-11,2%%time:~-8,2%%time:~-5,2%').trim())
                        try
                        {
                            bat (returnStdout: true, script: "@python3 -m venv $venv_name")
                            echo "+ Successfully created venv."
                        } catch(Exception er){
                            error('- Error happened while trying to create the venv.')
                        }
                        try
                        {
                            bat("\\$venv_name\\Scripts\\activate.bat") //going into the environment this command in equal to "Scripts\activate.bat" in linux
                            bat (returnStdout: true, script: 'pip install -r requirements.txt') //installing tools in the venv
                            echo('All OK...')
                        }catch(Exception err){
                            echo('Failed to installing some of requerments, Please check the log.')
                            error err.toString()
                        }
                    }
            }
        }
        stage('Deployment-stage'){
            steps{
                script {
                        echo '*Stage3 - Running webserver...*'
                        try
                        {
                            output = bat (returnStdout: true, script: 'uvicorn main:app --reload')
                            echo "+ Successfull."
                        }catch(Exception et){
                            error '- Failed, error was: ' + et.toString()
                        }
                    }
            }
        }
        stage('artifact-build-stage'){
            //in this stage the pipeline stores the python API app to an articat <m.Taghadosi>
            //and also copy that artifact to a repository like Nexus Or just a categorized folder.
            steps{
                //bat 'dir'
                //echo bat(returnStdout: true, script: 'set') //Environmental Variables - usefull sometimes <mt>
                script{
                    echo '*Stage4 - Trying to Artifact pythonAPI and export Artifact*'
                    try{
                        //Copying the artifact into the folder with unique name that i explained above
                        echo("xcopy /s .\\python_service\\* .\\artifacts\\$venv_name\\*")
                    }catch(Exception e){
                        error '- Failed to Copy artifact! The Error was: ' + e.toString()
                    }
                }
            }
        }
    }

    post{
        always{
            echo 'Attemping some cleanup...'
            echo ("rmdir /s /q $venv_name") //delete the venv folder 

            echo "${NAME}"
            //<m.Taghadosi>
        }
        success{
            echo 'The Pipeline execution was a SUCCESS.'
        }
        failure{
            echo 'Attemping to remove the not-healthy artifact'
            echo ("rmdir /s /q artifacts\\$venv_name") //delete the venv folder 
            echo 'Some stages failed, removing temp files.'
            //i want to add some cleanups later <m.Taghadosi>
        }
    }
}
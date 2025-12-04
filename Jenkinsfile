node{
    def appdir ="var/www/my-nextjsapp"

    stage("clean workspace"){
        echo "Cleaning  jenkins workspace"
        deleteDir()
    }

    stage("clone repo"){
        echo "Clonning the repo"
        git (
            branch: 'main',
            url: ''
        )

    }
    stage("deploy to Ec2"){
        echo "Deploying to EC2 instance"
        // here the 3 qutation define a multi line string for shell script
        sh """
         sudo mkdir -p ${appdir}
         sudo chown -R jenkins:jenkins ${appdir}  // give permission to jenkins user , give admin right to jenkins user

          rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appdir}                 //here the -a is for archive mode and -v is for verbose mode, v mean have to give details of log file , -a means remove duplicate , and --delete means delete the files which are not in source

          cd ${appdir}
          sudo npm install
          sudo npm run build
          sudo fuser -k 3000/tcp || true          // to kill the process running on port 3000 if it already running
          npm run start


            
            """
    }

}

// after run this go to visudo file and give permission to jenkins user to run npm without sudo password
// write this command under the root in visudo file
 // jenkins ALL=(ALL) NOPASSWD: All
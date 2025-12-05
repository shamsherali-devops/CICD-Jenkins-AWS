node{
    def appdir ="/var/www/my-nextjsapp"  

    stage("clean workspace"){
        echo "Cleaning  jenkins workspace"
        deleteDir()
    }

    stage("clone repo"){
        echo "Clonning the repo"
        git (
            branch: 'main',
            url: 'https://github.com/shamsherali-devops/CICD-Jenkins-AWS'
        )

    }
    stage("deploy to Ec2"){
        echo "Deploying to EC2 instance"
       
        sh """
         sudo mkdir -p ${appdir}
         sudo chown -R jenkins:jenkins ${appdir}  

          rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appdir}    

          cd ${appdir}
          sudo npm install
          sudo npm run build
          sudo fuser -k 3000/tcp || true       
          pm2 restart my-nextjsapp || pm2 start npm --name "my-nextjsapp" -- run start


            
            """
    }

}


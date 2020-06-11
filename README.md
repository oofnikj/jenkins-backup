# Jenkins Automatic Backup job

Adds couple of important Jenkins config items to Git and pushes them to a remote repository.

Currently backed up items:
- job config
- script approval
- plugins


## Initial setup on Jenkins host

- generate SSH key pair:
```
ssh-keygen -t ecdsa jenkins-backup.pem
```
- create a new backup repo
- add public key to GitHub
- add private key to Jenkins credentials

- on Jenkins master host, launch a Docker container and mount in `jenkins_home` from existing Jenkins container: 
```
docker run --rm -it --volumes-from jenkins_jenkins_1 \
  -v `pwd`/jenkins-backup.pem:/root/.ssh/id_ecdsa alpine sh
```

- initialize local repository and push to remote
```
apk update && apk add git openssh
cd /var/jenkins_home
git init .
git config user.name "Jenkins Backup"
git config user.email "jenkins-backup@noreply.com"
git remote add origin <git_remote_uri>
git commit -m "initial"
git push -u origin master
chown -R 1000:1000 .
```
- Add SSH private key to Jenkins credential store
- Create a new Pipeline job with the Pipeline configuration from [here](jenkins/pipeline/jenkinsBackup/Jenkinsfile)

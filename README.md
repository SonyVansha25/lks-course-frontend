# Course App - Frontend

## Environment Setup
> Create .env file in root folder
```sh
NUXT_ENV_API_URL=YOUR_BACKEND_URL # For example NUXT_ENV_CATALOG_API_URL=http://YOUR_ECS_LB
```

### Pre-Requirment
> If you are using Amazon Linux as Operating System you can install nodejs packages with this following command:
```sh
$ sudo yum install -y gcc-c++ make
$ curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash -
```

## Run the application
> This app has two options to run it.

### 1st Option - Server Deployment
```sh
# Install all Depedencies
$ npm install
# Firstly You need to run this command to create .nuxt directory with everything inside ready to start
$ npm run build 
# Start your client apps in server-side production mode
$ npm run start 
```

### 2nd Option - Static Deployment (Pre-rendered)
> Gives you the ability to host your web application on any static hosting (Nginx or Apache2 ect), the static source code will be generated in *dist folder*

```sh
# Install all Depedencies
$ npm install
# Generate static source code
$ npm run generate # Use --prefix <your_path> for specific path and use --quite or --slient for suppressing the output of npm
# Then you can use your web server enggine that you love to run it.
```

### UserData to use Instance Front end
```sh
#!/bin/bash
echo "step1 - install dependencies"
yum install -y gcc-c++ make git amazon-efs-utils
echo "step2 - install nodejs v16"
yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm -y
yum install -y nodejs
echo "step3 - Clone github Backend"
mkdir /home/ec2-user/apps-cloud-lksn2022
git clone https://github.com/SonyVansha25/apps-cloud-lksn2022.git /home/ec2-user/apps-cloud-lksn2022
echo $(ls /home/ec2-user/apps-cloud-lksn2022)
echo "step4 - Create env"
touch /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_HOST=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_USER=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_PASS=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_NAME=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "NODE_ENV=production\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_ACCESS_KEY=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_SECRET_KEY=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_BUCKET_NAME=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_HOST=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_PORT=6379\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_PASS=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "LOG_PATH=/home/ec2-user/efs/server/tmp\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "CACHE_PATH=/home/ec2-user/efs/server/logs\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
echo "step5 - mount efs"
mkdir /home/ec2-user/efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 180.10.2.22:/ /home/ec2-user/efs
echo "step6 - npm install package"
npm install pm2 --prefix /home/ec2-user/apps-cloud-lksn2022/backend
echo "step7 - npm install backend" 
npm install --prefix /home/ec2-user/apps-cloud-lksn2022/backend
npm run start-apps --prefix /home/ec2-user/apps-cloud-lksn2022/backend
echo "step8 - Done"
```

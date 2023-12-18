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
sudo su
echo "step1.install dependencies"
yum install git httpd gcc-c++ make -y
yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm -y
yum install nodejs -y
systemctl start httpd
systemctl enable httpd
echo "step2.clone git repo"
mkdir /home/ec2-user/lks-course-frontend
git clone https://github.com/betuah/lks-course-frontend.git /home/ec2-user/lks-course-frontend
echo $(ls /home/ec2-user/lks-course-frontend)
echo "step3.create environment"
touch /home/ec2-user/lks-course-frontend/.env
printf "NUXT_ENV_API_URL=http://lks-tg-frontend-1022595.us-east-1.elb.amazonaws.com" >> /home/ec2-user/lks-course-frontend/.env
echo "step4.install and run application"
npm install --prefix /home/ec2-user/lks-course-frontend/
npm run generate --prefix /home/ec2-user/lks-course-frontend/ -y
cp -R /home/ec2-user/lks-course-frontend/dist/* /var/www/html
echo "step5.create virtualhost"
touch /etc/httpd/conf.d/proxy.conf
printf "<Virtualhost *:80>\n" >> /etc/httpd/conf.d/proxy.conf
printf 'Header set Access-Control-Allow-Origin "*"\n' >> /etc/httpd/conf.d/proxy.conf
printf 'Header set Access-Control-Allow-Methods "GET, PUT, POST, DELETE, PATCH"\n' >> /etc/httpd/conf.d/proxy.conf
printf "ProxyPreserveHost On\n" >> /etc/httpd/conf.d/proxy.conf
printf "ProxyPass /api/ http://internal-lks-lb-backend-489163977.us-east-1.elb.amazonaws.com/api/\n" >> /etc/httpd/conf.d/proxy.conf
printf "ProxyPassReverse /api/ http://internal-lks-lb-backend-489163977.us-east-1.elb.amazonaws.com/api/\n" >> /etc/httpd/conf.d/proxy.conf
printf "</Virtualhost>\n" >> /etc/httpd/conf.d/proxy.conf
systemctl restart httpd
systemctl status httpd
echo finish
```

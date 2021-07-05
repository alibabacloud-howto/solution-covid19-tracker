# Cloud Native Web App on Alibaba Cloud: COVID-19 Tracker

This solution deploys simple cloud native web app (https://github.com/yankidank/covid-19-tracker) on Alibaba Cloud to visualize the spread and impact of COVID-19, subscribe for daily updates, and view stats.

![image desc](https://labex.io/upload/Y/Q/T/hTJ48P6KVnAM.jpg)

## Architecture
![image desc](https://labex.io/upload/P/W/H/PZpJIe60CxkY.png)

## 0. Index
- [1. Use Terraform to create resources](https://github.com/alibabacloud-howto/solution-covid19-tracker#1-use-terraform-to-create-resources)
  - [1.1 Install Terraform (skip this step if you already have Terraform installed)](https://github.com/alibabacloud-howto/solution-covid19-tracker#11-install-terraform-skip-this-step-if-you-already-have-terraform-installed)
  - [1.2 Create resources](https://github.com/alibabacloud-howto/solution-covid19-tracker#12-create-resources)
- [2. Install NodeJS](https://github.com/alibabacloud-howto/solution-covid19-tracker#2-install-nodejs)
- [3. Install the web app project](https://github.com/alibabacloud-howto/solution-covid19-tracker#3-install-the-web-app-project)
  - [3.1 Download project](https://github.com/alibabacloud-howto/solution-covid19-tracker#31-download-project)
  - [3.2 Import data](https://github.com/alibabacloud-howto/solution-covid19-tracker#32-import-data)
  - [3.3 Get Sparkpost key](https://github.com/alibabacloud-howto/solution-covid19-tracker#33-get-sparkpost-key)
  - [3.4 Run the web app](https://github.com/alibabacloud-howto/solution-covid19-tracker#34-run-the-web-app)

## 1. Use Terraform to create resources

### 1.1 Install Terraform (skip this step if you already have Terraform installed)

Set up terraform CLI environment on your desktop. For the tutorial about how to setup Terraform CLI, you can refer to https://partners-intl.aliyun.com/help/doc-detail/91289.htm

### 1.2 Create resources

Refer back to the user's home directory as shown below, click AccessKey Management.

![image desc](https://labex.io/upload/G/I/B/BUGx6Dcu78BY.jpg)

Click Create AccessKey. After AccessKey has been created successfully, AccessKeyID and AccessKeySecret are displayed. AccessKeySecret is only displayed once. Click Download CSV FIle to save the AccessKeySecret 

![image desc](https://labex.io/upload/H/F/B/8Y6UiUGsRpR3.jpg)

Go to the directory of Terraform script: ``deployment/terraform/``. Enter the command ``vim main.tf``, edit the Terraform script. ***Please pay attention to modify var.access_key and var.secret_key to the user's own AccessKey***

![image desc](https://labex.io/upload/C/D/E/z6l8hf3MUKiG.jpg)

Enter the following command,

```
terraform init
```

![image desc](https://labex.io/upload/V/U/V/qElZZrUrGGWM.jpg)

Enter the following command to list the resources planned to be created according to the template.

```
terraform plan
```

![image desc](https://labex.io/upload/Q/T/P/bogZAjU5CIqd.jpg)

Enter the following command to start creating resources based on the template.

```
terraform apply
```

![image desc](https://labex.io/upload/A/E/O/6xcgB9bQ4909.jpg)

Enter "yes" to start creating related resources. It takes about 3 minutes, please wait patiently.

![image desc](https://labex.io/upload/O/Y/N/C3xkE6oSJSXG.jpg)

Created successfully.

![image desc](https://labex.io/upload/H/V/V/WXfbipAkqSYz.jpg)

## 2. Install NodeJS

Back to the ECS console, you can see the new ECS instance you just created, and log in to the instance remotely.

![image desc](https://labex.io/upload/M/E/A/vfSoCVLmezNK.jpg)

> The default account name and password of the ECS instance:
> 
> Account name: root
> 
> Password: Aliyun-test

Enter the following command to download the nodejs installation package.

```
wget https://nodejs.org/dist/v14.17.1/node-v14.17.1-linux-x64.tar.xz
```

![image desc](https://labex.io/upload/D/E/F/DomTLWPsJeu7.jpg)

Enter the following command to decompress the installation package.

```
tar -xf node-v14.17.1-linux-x64.tar.xz
```

![image desc](https://labex.io/upload/C/R/F/xCJWpQczFqLY.jpg)

Enter the following command to create a command soft link.

```
ln -s /root/node-v14.17.1-linux-x64/bin/node /usr/bin/

ln -s /root/node-v14.17.1-linux-x64/bin/npm /usr/bin/
```

![image desc](https://labex.io/upload/U/M/F/GdCPjTfvj5v6.jpg)

Enter the following command to check the node version.

```
node -v

npm -v
```

![image desc](https://labex.io/upload/Q/X/X/8AWjgcstZDKj.jpg)

Has been successfully installed.

## 3. Install the web app project

### 3.1 Download project

Enter the following command to install git.

```
apt update && apt -y install git
```

![image desc](https://labex.io/upload/S/N/A/nXk8zcTWVdGB.jpg)

Enter the following command to pull the sample project.

```
git clone https://github.com/yankidank/covid-19-tracker.git
```

![image desc](https://labex.io/upload/K/B/A/3tAfRZdru4Pk.jpg)

Enter the following command to enter the project directory.

```
cd covid-19-tracker
```

![image desc](https://labex.io/upload/L/Y/E/IPGeMkbZs2lv.jpg)

Enter the following command to install project dependencies.

```
npm install --unsafe-perm
```

![image desc](https://labex.io/upload/R/E/X/VMQY4vCeWZO3.jpg)

### 3.2 Import data

Refer to the figure below to go to the Alibaba Cloud RDS console,

![image desc](https://labex.io/upload/S/P/X/jPftJyn3MV10.jpg)

You can see the RDS database just created using Terraform.

![image desc](https://labex.io/upload/H/P/A/q4LCtEJidYrp.jpg)

Click to enter the instance ID to enter the detailed page. You can see that the "covid19" database and account have been created automatically.

![image desc](https://labex.io/upload/F/U/B/JWzEm8qXXESN.jpg)

![image desc](https://labex.io/upload/K/L/T/4B4kiowVTGNZ.jpg)

Intranet address of RDS.

![image desc](https://labex.io/upload/L/Q/E/uftf318y3aPH.jpg)

Back to the ECS command line, enter the following command to install the mysql client.

```
apt -y install mysql-client
```

![image desc](https://labex.io/upload/P/N/C/zmDILEpbzvyZ.jpg)

Enter the following command to log in to the RDS instance database. ***Please pay attention to replace YOUR-RDS-INTERNAL-ENDPOINT with the intranet address of the user's own RDS instance***.

```
mysql -hYOUR-RDS-INTERNAL-ENDPOINT -ucovid19 -pAliyun-test -Dcovid19 
```

![image desc](https://labex.io/upload/V/J/P/kLDsZVi6XTPI.jpg)

Enter the following command to create the "stats" table.

```
CREATE TABLE stats
(
	id int NOT NULL AUTO_INCREMENT,
    FIPS INTEGER NULL,
    Admin2 VARCHAR(100) NULL,
    city VARCHAR(100) NULL,
    province VARCHAR(500) NULL,
    country VARCHAR(100) NULL,
    confirmed INTEGER default 0,
    deaths INTEGER default 0,
    recovery INTEGER default 0,
    last_update datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
    latitude Decimal(18,15) NULL,
    longitude Decimal(18,15) NULL,
    active INTEGER NULL,
    combined_key VARCHAR(500) NULL,
    primary key(id)
);
```

![image desc](https://labex.io/upload/X/D/B/fQZz5CrjKSdL.jpg)

Enter the following command to exit the database first.

```
exit
```

![image desc](https://labex.io/upload/K/E/R/0WpAX9pBTVDZ.jpg)

Enter the command ``vim config/config.json`` to open the configuration file and modify the connection configuration of the database referring to the following figure.

![image desc](https://labex.io/upload/N/X/A/2kDBULlk08gk.jpg)

Enter the command ``vim csv_to_mysql/read-file-4.js`` to open the configuration file and also modify the connection configuration of the database.

![image desc](https://labex.io/upload/A/J/W/7UgULgmZkwps.jpg)

Enter the following command to execute the script file to import data into the database.

```
cd csv_to_mysql

node read-file-4.js
```

![image desc](https://labex.io/upload/Y/D/A/Qt2i8x6d2iwO.jpg)

![image desc](https://labex.io/upload/E/D/E/GNNYqFQNk0pt.jpg)

### 3.3 Get Sparkpost key

SparkPost is a developer-centric email delivery service. All email sending and receiving can be done using API.

Users need to log in to the Sparkpost official website to register an account.

```
https://www.sparkpost.com/
```

![image desc](https://labex.io/upload/U/L/K/0pNqMPJY1f4l.jpg)

Refer to the figure below to create a KEY.

![image desc](https://labex.io/upload/K/G/F/ZeZBMGMCwyor.jpg)

### 3.4 Run the web app

Back to the ECS command line,

Enter the command "vim /etc/profile" and add the following content to the end of the file. ***Please pay attention to replace YOUR-API-KEY and YOUR-SECRET with your own***.

```
vim /etc/profile
```

```
export SPARKPOST_API_KEY=YOUR-API-KEY
export LOGIN_SECRET=YOUR-SECRET
```

![image desc](https://labex.io/upload/I/L/G/ufNXXKCMIrU2.jpg)

Enter the following command to make the modification effective.

```
source /etc/profile
```

![image desc](https://labex.io/upload/Q/N/O/ltN3GfDxmCwl.jpg)

Enter the following command to start the application.

```
cd /root/covid-19-tracker

node server.js
```

![image desc](https://labex.io/upload/L/V/W/IH0VvF0Fq2Fn.jpg)

Refer to the figure below and visit on the browser with ECS public IP:

```
http://<ECS_IP>:8080
```

![image desc](https://labex.io/upload/K/B/V/c3mxkXKnRtj0.jpg)

It shows that the startup is successful.

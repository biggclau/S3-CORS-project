####   STEPS TO CREATE A SAMPLE EC2 WEBSITE WITH CORS ENABLE TO PULL DATA FROM S3 BUCKET WEBSITE CORS configuration FOR S3

Cross-origin resource sharing (CORS) defines a way for client web applications that are loaded in one domain to interact with resources in a different domain. With CORS support, you can build rich client-side web applications with Amazon S3 and selectively allow cross-origin access to your Amazon S3 resources.
This project will show you how to enable CORS using the Amazon S3 console to enable a website hosted on an ec2 instance to make call to fetch data from an S3 static website.

To configure your bucket to allow cross-origin requests, you add a CORS configuration to the bucket. A CORS configuration is a document that defines rules that identify the origins that you will allow to access your bucket, the operations (HTTP methods) supported for each origin, and other operation-specific information. 
 
    NB S3 new console only supports JSON for CORS configuration so if you use another language, configuration will fail.

###  SCENARIO 1

Instead of accessing a website by using an Amazon S3 website endpoint, we will enable CORS and serve the content through our static website url (webpage link) within our html code in our index.html file.


### **TO GET STARTED**

- Log on to your aws console and  spin up a Linux server with Apache installed, started, enabled using a userdata script below.  Make sure your ec2 instance have a role attached to it that has AmazonS3FullAccess and AmazonSSMFullAccess policies attached to it. Note that thsi instance must be able to serve traffic from the internet since it will be used to host a public facing website.


'''
#!/bin/bash
sudo yum install -y httpd.x86_64
sudo yum update -y httpd.x86_64
sudo systemctl start httpd.service
sudo systemctl enable httpd.service
'''

- Connect into your EC2 instance and move to the apache home directory 

'''
cd /var/www/html
'''

- Create an S3 bucket and create a static website from it.Upload all files I shared on team under S3 project folder for CORS into S3 bucket. Then run the following command on on your EC2 instance to copy the index.html file into /var/www/html folder.
NB- Replace "S3-bucket-index.html-file-path" in the code below with your actual index.html location path in your S3 bucket.
mine looks like this '''sudo aws s3 cp s3://tekgloballlc.com/s3website/index.html /var/www/html/'''

'''
sudo aws s3 cp "S3-bucket-index.html-file-path" /var/www/html/
'''

copy public ip adddress of EC2 instance and check website content on browser. it will display part of your content but it will not Display the content found in your "sample.json" file in your S3 static website.



- To verify this , In your web brouser: open up 
'''
>more tools>developers tools
'''

![IMG-2205](https://user-images.githubusercontent.com/73201241/202801356-2a592da2-17f0-4eb7-a115-a22c3e501f54.jpg)


- select console and at the bottom of the page you will see a failed CORS request massage. this is to let you know that the website you accessed is not displaying all massages because it was unable to access some data from an s3 static website. 


By default, all http/https setting block any CORS headers from accessing data in your S3 static website. You will need to configure CORES on your S3 bucket to enable access for your EC2 website to pull all data.

    NB S3 new console only supports JSON for CORS configuration so if you use another language, configuration will fail.

To complete the configuration, go to the permision page of your S3 bucket and scroll all the way down to the CORS setting. Click on edit and copy and paste below json code to allow all HTTP/HTTPS headers to be able to access data in your S3 static website page.

'''
[
    {
        "AllowedHeaders": [
            "Authorization"
        ],
        "AllowedMethods": [
            "GET",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "Access-Control-Allow-Origin"
        ]
    }
]
'''


Save CORS setting and refresh your website. All data from S3 static website file you refrence in your index.html for your website will be displayed. You can make changes to the content of the Sample.json file in your S3 bucket and refresh your website and see how content in website will change.

this is a simple way to ilustrate how most companies store application data in S3 and use CORES and EC2 application hosting to pull the data and display as content on their website.

Any questions?







# Integration-of-s3-lambda-and-api-gateway
Using Api gateway to upload fiile in s3  and then s3 will trigger lambda function and we can see that in cloudwatch logs group


Let's first create lambda function go to lambda service and click on create function and give a function name let say "hello_world" and click on create

![image](https://github.com/IshikaSahu/Integration-of-s3-lambda-and-api-gateway/assets/71627396/87b1674a-cafc-41e0-a502-e08aa3e13448)

Now come down and write your code and deploy it. 

Let's now create a bucket in s3 aws cloud service

So for creating a bucket we need to go to s3 service and click on create bucket a page will look like this:
![image](https://github.com/IshikaSahu/Uploading-objects-to-S3-using-API/assets/71627396/10159c0f-8416-44a5-a900-ab4801b06c56)
give your unique bucket name let say "ishika-b-11" and click on create bucket.
![image](https://github.com/IshikaSahu/Integration-of-s3-lambda-and-api-gateway/assets/71627396/6bc10f5d-9a74-4e3c-8b75-e31b489aee72)

Now we will create Event listener which will call lambda function as we upload anything s3

Now go inside your bucket that you have created right now and then go in properties and come down you will see there is one property Event Notifications so create new Event Notification.
Give event name and then choose put inside event type and destination lambda function and choose function name and then save changes.

![image](https://github.com/IshikaSahu/Integration-of-s3-lambda-and-api-gateway/assets/71627396/016aed02-851e-4e01-9897-12f1ecb2f149)

Now let's go and make an api gateway to upload object in s3 using api

Go to service api gateway in aws cloud and click on create api and then click on build button of 3rd option Rest Api. The page will look like this
give api name and click on create and then go on action option and create resource. Give resource name and then click on create button then create another resource and give variable over there i gave it {myfile} and click on create. Now we will create method for uploading file using api then go to action and click on create method and choose put and click on tick button.
Now choose 
Integration type : aws service
aws region : bucket region
aws service : s3
HTTP method : PUT
Action Type : Use path override
path override : bucket-name/{variable} 

Now here one problem came as api will hit there will be somthing that we are going to upload so that means api gateway need to contact s3 service for which there is required a role to create between them for authentication purpose so we will now go on IAM service in aws cloud and then we will go on roles and create a new role. We will choose api gateway in use case and click on next again go to next and give role name and click on create role now go inside of your role. Now click on add permissions and the attach policies and the attach a policy name AmazonS3FullAccess, click on add permissions.

![Screenshot 2023-06-30 234100](https://github.com/IshikaSahu/Integration-of-s3-lambda-and-api-gateway/assets/71627396/f78dbea4-9de5-4f13-bf6c-9aa8ded8b6d9)


Now copy arn number of that role and come back to api gateway service and paste the value of 
execution role : arn:_______
click on save 

now come on integration service and go down you will find url path parameters add path over there
Name                           Mapped from
variable name (method)         method.request.path.myfile
click on tick button
now go in settings and come down you will see one option called binarymedia types give there */* which means accept files of all type and click on save changes
Now come on resources again and go to actions

![image](https://github.com/IshikaSahu/Integration-of-s3-lambda-and-api-gateway/assets/71627396/4ae6cc39-cd98-4695-8f78-0782e212230d)
Now just deploy it and run it using curl command and upload any file to run lambda function and watch in CloudWatch in log groups whether lambda function executed or not


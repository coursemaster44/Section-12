# Section-12

# Section-12-Deploying-Sample-App-With-CRUD-Functionality-on-AWS-(With-CodePipeline)

**Step 1.Goto AWS Management Console>Services>Developers Tools>CodePipeline>Create Pipeline**

**Step 2.In Pipeline Settings give following details:**
- Pipeline name - cp-crud
- Service role - Existing service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step 3.Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - crud-app
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 4.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - crud-cb
- Build type - Single build

Click on Next

**Step 5.Deploy-optional in Add deploy stage**
- Deploy provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Application name - cd-app-cd
- Deployment group - cd-app-cd-dg 

Click on Next

**Step 6.Review all stages**
- Click on Create pipeline

**Step 7. The pipeline has been created**
- See that Source stage in progress


**Step 8.Goto Ec2>Security Groups>sg-xxxx>Edit inbound rules**
- Protocol:Port = Tcp:8080 from Source(0.0.0.0/0)

Click on Save
**Step 9.Copy the Public ip address and paste in browser to see that Application running**
 
**Step 10.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following command
```sh
$ git status
$ git add .
$ git commit -m "changed index color for cp "
$ git push
```

**Step 11.Go back to see the change in Pipeline**
- Pipeline is activated Just now
- See the latest commit - "changes index color for cp "
- Monitor all stages Build>Source>Deploy

**Step 12.Open Postman Tool>Environment>Manage Environments**
- Select ec2 environment and update the url current Value as follows:
- e.g.13.222.125.52:8080

Click on Update and exit

**Step 13.Now select ec2 as Environment**

**Step 14.Goto Pipeline Deployment is successful now**

**Step 15.Now select {{url}}/create table and Click on Send**
- Table created successfully

**Step 16.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
  - Table is created

**Step 17.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/readData
- Click on Send

**Step 18.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/insertData
- Click on Send

**Step 19.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 20.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 21.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 22.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 23.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data deleted

**Step 24.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 25.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted

# End of Lab


# 3-crud-ebs-single-ec2-deployment-lab

**Step 1.Goto Pipeline>pipeline-cp-ebs>edit**
- Click on Add Stage
  - Stage name - Pre-prod
  - Click on add stage

**Step 2.Pre-prod>Add action group**
- Action name - Approval-1
- Action provider - Manual approval
- SNS topic ARN - arn:aws:sns:ap-south-1xxxxx-Topic
- URL for review - Copy Sample-node-env URL

Click on Done

**Step 3.Pre-prod>+ Add action group**
- Action name - single-ec2-deployment
- Action provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Input artifacts - BuildArtifact
- Application name - crud-app-cd
- Deployment group - cd-app-cd-dg

- Click on Done

Click on save to save pipeline changes

**Step 4.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following command
```sh
$ git status
$ git add .
$ git commit -m "changed color of about for manual approval-1 "
$ git push
```

**Step 5.Goto your Email-Inbox to approve the Pre-prod stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-1

Click on link to Approve

**Step 6.Go back to Pipeline and click on Review in Pre-prod stage**
- Give approval comment 

Click on Approve

**Step 7.Pre-prod "single-ec2-deployment" started after approval**

**Step 8.Open Postman Tool>Environment>Manage Environments**

**Step 9.Now select ec2 as Environment and change its url value to ec2's IP**

**Step 10.select {{url}}/create table and Click on Send**
- Table created successfully

**Step 11.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
- Table is created

**Step 12.Goto Postman Tool and select ALB as Environment**
- Put the following value - http://{{url}}/insertData
- Click on Send

**Step 13.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 14.Goto Postman Tool and Now select ALB as Environment**
- Put the following value - http://{{url}}/readData
- Click on Send

**Step 15.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 16.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 17.Goto Postman Tool and Now select ALB as Environment**

- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 18.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**

Check for the data deleted
**Step 19.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 20.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted

# End of Lab



# 4-crud-cp-ebs-deployment-lab

**Step 1.AWS Console>Services>Elastic Beanstalk>Create Application**
- Create a web app
  - Application Name- crud-app-ebs
  - Platform 
    - Platform 
      - Node.js
   - Platform branch
      - Node.js 12 running on 64bit Amazon Llinux2
   - Platform version
      - 5.2.3(Recommended)
   - Application Code - Select Sample application

Click on Create Application.

**Step 2. Creating environment started**
- Elastic BeanStalk launches an environment named crudappebs-env
- Click on crudappebs-env to see it running

**Step 3.AWS Console>Developers Tools>Pipeline>Pipelines>Create Pipeline**
In Pipeline Settings give following details:
- Pipeline name - cp-ebs
- Service role - Select Existing service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step 4.Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - crud-App
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 5.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - crud-cb
- Build type - Single build

Click on Next

**Step 6.Deploy-optional in Add deploy stage**
- Deploy provider - AWS Elastic BeanStalk
- Region - Asia Pacific(Mumbai)
- Application name - crud-app-ebs
- Deployment group - crudappebs-env

Click on Next

**Step 7.Review all stages**
- Click on Create pipeline

**Step 8. The pipeline cp-ebs has been created**
- Monitor the progress of all the stages Build>Source>Deploy

**Step 9 Deployment has been completed** 
- Refresh the crudappebs-env link 
- See that it is running


**Step 10.Open Postman Tool>Environment>Manage Environments**

**Step 11.Now select ebs as Environment and change its url value to "crudappebs-env link"**

**Step 12.select {{url}}/create table and Click on Send**
- Table created successfully

**Step 13.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
- Table is created

**Step 14.Goto Postman Tool and select ALB as Environment**
- Put the following value - http://{{url}}/insertData
- Click on Send

**Step 15.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 16.Goto Postman Tool and Now select ALB as Environment**
- Put the following value - http://{{url}}/readData
- Click on Send

**Step 17.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 18.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 19.Goto Postman Tool and Now select ALB as Environment**

- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 20.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**

Check for the data deleted
**Step 21.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 22.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted

**Step 23.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following command
```sh
$ git status
$ git add .
$ git commit -m "changed index color for cp-ebs "
$ git push
```

**Step 24.Go back to our pipeline and see that it is started for change**
- See the latest commit - "changed index color for cp-ebs "
- Monitor all stages Build>Source>Deploy

**Step 25.Deployment has been completed** 
- Refresh the crudappebs-env link
- See that color is changed

# End of Lab

# 5-crud-ebs-single-ec2-multiple-ec2-lab

**Step 1.Goto Pipeline>pipeline-cp-ebs>edit**
- Click on Add Stage 
  - Stage name - Production
  - Click on add stage

**Step 2.Production>Add action group**
- Action name - Approval-2
- Action provider - Manual approval
- SNS topic ARN - arn:aws:sns:ap-south-1xxxxx-Topic

Click on Done

**Step 3.Production>+ Add action group**
- Action name - fleet-of-ec2-instances-deployment
- Action provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Input artifacts - BuildArtifact
- Application name - crud-app-cd
- Deployment group - crud-app-cd-asg-dg

- Click on Done

Click on save to save pipeline changes

**Step 4.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following command
```sh
$ git status
$ git add .
$ git commit -m "changed index and about color for cp-production-deployment "
$ git push
```
**Step 5.Go back to our pipeline and see that it is started for change**
- See the latest commit - "changed index and about color for cp-production-deployment "
- Monitor all stages Build>Source>Deploy

**Step 6.Deployment has been completed** 
- Refresh the crudappebs-env link
- See that color is changed

**Step 7.Goto your Email-Inbox to approve the Pre-prod stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-1

Click on link to Approve

**Step 8.Go back to Pipeline and click on Approval-1>Review in Pre-prod stage**
- Give approval comment 

Click on Approve

**Step 9.Pre-prod "single-ec2-deployment" started after approval**
- Production stage id pending for approval

**Step 10.Goto your Email-Inbox again to approve the Production stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-2

Click on link to Approve

**Step 11.Go back to Pipeline and click on Approval-2>Review in Production stage**
- Give approval comment 

Click on Approve

**Step 12.Production "fleet-of-ec2-instances-deployment" started after approval**

**Step 13 Deployment is in progress now**

**Step 14.After the deployment is completed run APIs**

**Step 15.Open Postman Tool>Environment>Manage Environments**

**Step 16.Now select ALB as Environment and change its url value to ALB DNS name**

**Step 17.select {{url}}/create table and Click on Send**
- Table created successfully

**Step 18.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
- Table is created

**Step 19.Goto Postman Tool and select ALB as Environment**
- Put the following value - http://{{url}}/insertData
- Click on Send

**Step 20.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 21.Goto Postman Tool and Now select ALB as Environment**
- Put the following value - http://{{url}}/readData
- Click on Send

**Step 22.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 23.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 24.Goto Postman Tool and Now select ALB as Environment**

- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 25.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**

Check for the data deleted
**Step 26.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 27.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted


# End of Lab










<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a CI/CD Pipeline with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codepipeline-updated)

**Author:** Szymon Wlach  
**Email:** szymonwlach.tech@gmail.com

---

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Introducing Today's Project!

In this project, I will demonstrate how to connect all parts of my CI/CD workflow into one fully automated pipeline using CodePipeline. I’m doing this project to learn how to automate the full process from code change to deployment without manual steps.

AWS CodePipeline is a service that connects tools like CodeBuild and CodeDeploy into one automated workflow. We are using it because, without it, I would have to manually build the project, select artifacts, and start deployments every time the code changes. CodePipeline automates these steps — it detects changes in the repository, triggers a build, deploys the new version, and handles failures — turning separate tools into a complete CI/CD system.

### Key tools and concepts

Services I used were AWS CloudFormation, AWS CodePipeline, AWS CodeBuild, AWS CodeDeploy, Amazon EC2, Amazon VPC, Amazon S3, IAM, CodeArtifact, and GitHub.  

Key concepts I learnt include Infrastructure as Code with CloudFormation, automating the creation and deployment of resources, connecting multiple CI/CD services into a single pipeline, passing artifacts between stages, enabling webhook triggers from GitHub, configuring service roles and permissions, monitoring pipeline executions, handling rollbacks, and validating automated deployments in a real environment.

### Project reflection

This project took me approximately 2 hours. The most challenging part was connecting all the CI/CD services correctly and ensuring the pipeline ran smoothly without errors. It was most rewarding to see a code change automatically trigger the build and deployment, completing the full CI/CD workflow successfully.

---

## Starting a CI/CD Pipeline

AWS CodePipeline is a fully managed CI/CD service that automates the process of building, testing, and deploying applications by connecting different stages of the development workflow into one continuous pipeline.

CodePipeline offers different execution modes based on how pipeline runs are handled when multiple changes happen close together. I chose the superseded mode, which replaces any previous in-progress or pending execution with the most recent one. Other options include processing executions one by one in order or allowing parallel executions, depending on how you want updates to be managed.

A service role gets created automatically during setup so the service has the necessary permissions to access and manage other AWS resources required to run the pipeline successfully.

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_gdnhtm)

---

## CI/CD Stages

✍️ The three stages I’ve set up in my CI/CD pipeline are:

Source – connects to my GitHub repository and monitors the default branch for changes.

Build – uses CodeBuild to compile and test the code from the Source stage.

Deploy – uses CodeDeploy to deploy the built artifacts to my EC2 instance

CodePipeline organizes the three stages into a visual workflow, showing the sequence from Source to Build to Deploy. In each stage, you can see more details on the current status, any errors that occurred, which artifacts are being used, and the duration of each action, giving you full visibility into the progress and health of your CI/CD pipeline.

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Source Stage

The purpose of specifying a default branch in the Source stage is to tell CodePipeline which branch to monitor for changes, so that commits to that branch automatically trigger the pipeline and start the CI/CD workflow.

The source stage is also where you enable webhook events, which allow CodePipeline to automatically detect changes in your repository and start the pipeline immediately.

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_sergt)

---

## Build Stage

SourceArtifact is the input artifact for the build stage because it contains the latest code (ZIP file)  from the repository that needs to be built and tested. Without this artifact, CodeBuild wouldn’t know what code to compile or process.

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_j1k2l3m4)

---

## Deploy Stage

The Deploy stage is where CodeDeploy takes the build artifacts from the Build stage and deploys them to the target environment, such as my EC2 instance. I configured it to use my existing CodeDeploy application and deployment group so that the latest version of my application is automatically deployed whenever the pipeline runs.

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_m4n5o6p7)

---

## Success!

Since my CI/CD pipeline gets triggered by changes in the GitHub repository, I tested it by adding a new line in the <body> section of index.jsp:

<p>If you see this line, that means your latest changes are automatically deployed into production by CodePipeline!</p>

I then saved the file, committed the change, and pushed it to the master branch of my GitHub repository. This triggered the pipeline to automatically build and deploy the updated application.

The moment I pushed the code change, my pipeline automatically started a new execution. The commit message under each stage reflects the changes I made, so I could track which version of the code was being built and deployed.

I clicked on the Source stage in the pipeline diagram and checked the commit message in the stage details panel. Clicking the Commit ID opened the commit page in GitHub, confirming that the new line in index.jsp was included. I then waited for the Build and Deploy stages to complete successfully, turning green, which verified that the pipeline deployed my changes automatically.

Once my pipeline executed successfully, I checked my web application in the browser and confirmed that the new line I added to index.jsp appeared on the page. This verified that the code change had been automatically built and deployed by CodePipeline, completing the full CI/CD workflow.

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_e1f2g3h4)

---

## Testing the Pipeline

In a project extension, I initiated a rollback on the Deploy stage of my pipeline after intentionally introducing a broken change. Automatic rollback is important for quickly restoring the application to a stable state, minimizing downtime, and ensuring that users aren’t affected by failed deployments.

During the rollback, the source and build stages are unaffected because the rollback only reverses the changes deployed in the Deploy stage; it doesn’t change the code in the repository or rebuild artifacts. I could verify this by comparing the commit history and build artifacts before and after the rollback, which remained the same.

After rollback, the live web app reverted to the previous version, and the new line I had added in index.jsp was no longer visible. This confirmed that the rollback successfully restored the application to its last stable state.

![Image](http://learn.nextwork.org/sympathetic_maroon_playful_teaberry/uploads/aws-devops-codepipeline-updated_sdfgsdfgdf)

---

---

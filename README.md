# Project Overview: Jenkins + Ansible

## Summary

This project aims to streamline the deployment process of a Java using Jenkins and Ansible, ensuring smooth transitions from code changes to production deployment. The workflow involves code modifications, triggering Jenkins pipelines, code analysis with SonarQube, Maven build with dependencies fetched from Nexus, artifact deployment on staging and production environments using Ansible, and productivity testing.

## Workflow Details

### Stage Environment Setup

1. **Create EC2 for Application Deployment**:
   - Spin up an EC2 instance to host the application.

2. **Security Configuration**:
   - Allow SSH (port 22) access from Jenkins (Ansible) to the application EC2.
   - Allow communication on port 8081 from the application EC2 to Nexus EC2.

3. **DNS Configuration**:
   - Create a private hosted zone (`vprofile.project`) on Route53.
   - Add an A record (`app01stg`) pointing to the private IP of the application EC2.

4. **Jenkins Configuration**:
   - Manage Jenkins credentials to add SSH username (`ubuntu`) and private key (associated with the keypair of the application EC2).
   - Install Ansible on the Jenkins server.
   - Install Ansible plugin on Jenkins portal.
   - Add Nexus credentials in Jenkins credentials.

5. **Ansible Playbooks**:
   - **Prepare**: Install JDK, download Tomcat tar, create user and group for Tomcat, extract Tomcat tar, configure ownership, set up Tomcat using a template based on the operating system.
   - **Deploy**: Download artifact from Nexus, execute deployment actions (backup current artifact, delete current artifact, deploy new artifact), start Tomcat service.

6. **Jenkins Pipeline Configuration**:
   - Configure Ansible settings.
   - Integrate pipeline execution stage using Ansible plugin (using entered Jenkins credentials).

7. **Inventory Configuration**:
   - Add `app01stg.vprofile.project` to the Ansible inventory.

8. **Jenkins Job Creation**:
   - Create a Jenkins job for staging environment based on the configured pipeline.

### Production Environment Setup

1. **Create Production EC2**:
   - Create a production EC2 instance with the same security group settings as the staging environment.
   - Add its SSH key on Jenkins (Ansible).

2. **Branch Creation**:
   - Create a production Git branch.
   - Create another Ansible inventory file for production EC2.

3. **Pipeline Adjustment**:
   - Remove build-related configurations from the Jenkins pipeline.
   - Prompt user input for artifact information to fetch from Nexus.

4. **Jenkins Job Creation**:
   - Generate a Jenkins job for the production environment based on the updated pipeline.

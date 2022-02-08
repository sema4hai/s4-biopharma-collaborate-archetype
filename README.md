# s4-biopharma-collaborate-architype
The repo contains code for the analysis of lysosomal storage diseases in our database. It is intended to be an example for collaborative data science so it will not be used to support an actual project. 

The repo uses Docker and OneDrive/SharePoint to allow code sharing and data sharing during a data science project. It uses a docker compose file to start an RStudio server app and/or Jupyter Notebook app. The approach ensures that everyone has the identical development environment, including R/Python versions and third party packages. Both the code directory and data directory are mounted to the Docker containers, allowing everyone using the same absolute file path during development and maximize reproducibility with minimal code refactoring. 

To use the repo structure for an actual project, copy all the files within this repo (including .gitignore but excluding .git) to your project repo, or download the latest release and rename the folder as desired.

## How to use

Start RStudio and/or Jupyter Notebook with Docker. Install Docker Desktop if you do not have it installed yet. Also install OneDrive and keep it running. You should have been shared with a OneDrive folder called "Biopharma_Shared_Workspace", which is a shared workspace for exchanging data.

1. Replace the placeholders in "compose_template.yaml" with your information and save the file as "compose.yaml". Do not change the "compose_template.yaml" file (except if you really want to contribute to it). Never put your database credentials into "compose_template.yaml".  

2. In your terminal, cd into the current repository and run the following command to start RStudio and/or Jupyter Notebook. 
    ```bash
    # start a container for RStudio
   docker compose up --build rstudio
    # or start a container for Jupyter Notebook
   docker compose up --build jupyter
   # or start both
   docker compose up --build
    ```
3. In your browser, access RStudio at "localhost:8787"; access Jupyter Notebook by the url printed in the terminal. 
  
   You may start writing the analysis codes. Remember to save them before closing your browser, otherwise you lose your changes. Access the database with the database credentials specified in the docker compose.yaml file. Save your code to "/s4-biopharma-lsd" and your can write results to be shared with colleagues at "/Biopharma_Shared_Workspace". 

4. To stop RStudio and/or Jupyter Notebook services, use "command + C". Then run the following command to remove the containers. 
   ```bash 
   docker compose down
   ```
   
5. Check in your code to Github in your terminal. You can do this step immediately after you saved code during developing (actually you should do this step frequently)

### What if I need to install a R/Python package?
Install R or Python packages in the corresponding Dockerfiles: `Dockerfile_custom_[RStudio|Jupyter]`.  This will give us an overview of frequently used packages and make it possible to build pre-built images for future projects. 


### How to set up port forwarding?
In your terminal, run the following command to start port forwarding:
```bash 
ssh -N -L 2345:{database url}:{database port, e.g. 5439} -i {path to your private key} {user name on bastion host}@{url of bastion host}
```
Keep it running while you are developing your code. Check the process is not broken if you fail to log into the database. 

### What if my file path has spaces in them?
Docker compose does not work well for file paths with spaces in them. In this case, you can create a soft link to your paths with the following script:
```bash 
ln -s {original path} {new path}
```
For details, read the manual at `man ln`.

### How to mount SharePoint folder in a Docker image?
Log into Office 365, open OneDrive and from there find a SharePoint site on the left navigation part. Click on the folder that you want to use (e.g. HAI/Project/Biopharma_Shared_Workspace), then you will be able to see "sync". Once you click sync, you can follow the instructions to track the folder on your local computer. Now you can mount it in a Docker image (create a soft link if the file path contains spaces). Be reminded that SharePoint folders are accessible to everyone while OneDrive folders are only accessible to invited collaborators. 

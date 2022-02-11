# s4-biopharma-collaborate-architype
The repo contains code for the analysis of lysosomal storage diseases in our database. It is intended to be an example for collaborative data science to maximize code reproducibility so it will not be used to support an actual project. 

How to define code "reproducibility"? According to [Best Coding Practices to Ensure Reproducibility](http://griverorz.net/assets/pdf/good_practices-pst.pdf),

> In the context of statistics and data science, reproducibility means that our code--a map from data to estimates or predictions--should not depend on the specific computational environment in which data processing and data analysis original took place.

Therefore, the goal of reproducible code is to allow code developed by one colleague on one laptop seamlessly run on another laptop. To do this, one wants to make sure code are shared, data access are granted and/or data are shared, software (R/Python) versions are consistent, dependencies are taken care of. 

The repo uses Docker and OneDrive/SharePoint to allow code sharing and data sharing during a data science project. It uses a docker compose file to start an RStudio server app and/or Jupyter Notebook app. The approach ensures that everyone has the identical development environment, including R/Python versions and third party packages. Both the code directory and data directory are mounted to the Docker containers, allowing everyone using the same file paths during development and maximize reproducibility with no code refactoring. 

To use the repo structure for an actual project, copy all the files within this repo (including .gitignore but excluding .git) to your project repo, or download the latest release and rename the folder as desired.

## How to use

Install Docker Desktop and OneDrive if you do not have them installed yet. Start Docker and OneDrive, and keep them running. You should have been shared with a OneDrive folder called "Biopharma_Shared_Workspace", which is a shared workspace for exchanging data. An alternative to OneDrive is using SharePoint folder "HAI/Project/Biopharma_Shared_Workspace". The team needs to decide which workspace to use at the start of a project. 

1. Replace the placeholders in "compose_template.yaml" with your information and save the file as "compose.yaml". Do not change the "compose_template.yaml" file (unless you really want to contribute to it, which is always welcomed). Never put your database credentials into "compose_template.yaml" (git tracks the template file but not "compose.yaml").   

3. In your terminal, cd into the current repository and run the following command to start RStudio and/or Jupyter Notebook. 
    ```bash
    # start a container for RStudio
   docker compose up --build rstudio
    # or start a container for Jupyter Notebook
   docker compose up --build jupyter
   # or start both
   docker compose up --build
    ```
   You can ignore the "--build" option if your Dockerfiles are not changed. This can save you some time to start the services. 
4. In your browser, access RStudio at "localhost:8787"; access Jupyter Notebook by at "localhost:8888" or the printed url in the console. 
  
   You may start writing the analysis codes. Remember to save them before closing your browser, otherwise you may lose your changes. Access the database with the database credentials specified in the docker compose.yaml file. Save your code to the repo, and write intermediary results in the /out folder. Also remember to refer to repo files by relative paths. For results to be shared with colleagues, write them at "/Biopharma_Shared_Workspace" with absolute paths. 

5. To stop RStudio and/or Jupyter Notebook services, use "command + C". Then run the following command to remove the containers. 
   ```bash 
   docker compose down
   ```
   
6. Check in your code to Github in your terminal. You can do this step immediately after you saved code during developing (actually you should do this step frequently)

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

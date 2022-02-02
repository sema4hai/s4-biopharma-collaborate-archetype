# s4-biopharma-lsd
The repo contains code for the analysis of lysosomal storage diseases in our database. 

## How to contribute

Start RStudio and/or Jupyter Notebook with Docker. Install Docker Desktop if you do not have it installed yet. Also install OneDrive and keep it running. You should have been shared with a OneDrive folder called "Biopharma_Shared_Workspace", which is a shared workspace for exchanging data.

1. Replace the placeholders in "compose_template.yaml" with your information and save the file as "compose.yaml". 

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

# Experiments with Kubeflow 

## What does Kubeflow do?

Kubeflow runs your workspaces. You can have notebook servers (called Jupyter
Servers), and in them you can create analyses in R and Python with interactive
visuals. You can save & upload data, download it, and create shared workspaces
for your team.

![Kubeflow Manages the Jupyter Servers](../images/jupyter_in_action.png)

**Let's get started!**

## Create a Server

### Log into Kubeflow

- Log into [the azure portal](https://portal.azure.com) **using your
  cloud.statcan credentials**.
 
??? warning "Log into the Azure Portal using your Cloud Credentials"
    You have to login to the azure portal **using your statcan credentials**.
    `first.lastname@cloud.statcan.ca`. You can do that here [at the azure portal](https://portal.azure.com).
    ![Azure Portal: Choose the `@cloud.statcan.ca` address](../images/azure-login.png)


- After logging into Azure, log into [kubeflow](https://kubeflow.covid.cloud.statcan.ca)


??? failure "Why am I getting "Missing url parameter: code"?"
    If you try to log into kubeflow and you get the message 
    > Missing url parameter: code

    It is because you are signed in with the wrong Azure account. You must sign
    in with your cloud credentials.

    ![This means you're in the wrong account](../images/missing_parameter_code.png)
    






- Navigate to the Jupyter Servers tab

![Kubeflow Manages the Jupyter Servers](../images/kubeflow_ui.png)

- Then click **+ New Server**

### Configuring your server

- You will get a template to create your notebook server.
  **Note:** the name must be lowercase letters with hypens. **No spaces, and no underscores.**


- **You'll need to choose an image** You will probably want one of

    - **Machine Learning**
    - **Geomatics**
    - **Minimal**

![Choose an Image](../images/kubeflow_choose_an_image.png)

- If you want to use a gpu, check if the image says **cpu** or **gpu**.
 
 
### CPU and Memory 

- At the time of writing (April 21, 2020) there are two types of computers in
  the cluster

  - **CPU:** D8s v3 `(8 vcpus, 32 GiB memory)`
  - **GPU:** NC6s_v3 `(6 vcpus, 112 GiB memory)`
  
  Because of this, if you request too much RAM or too many CPUs, it may be hard
  or impossible to satisfy your request.
  
  In the future (possibly when you read this) there may be larger machines made
  available, so you may have looser restrictions.
  
!!! note "Use GPU machines responsibly"
    There are fewer GPU machines than CPU machines, so use them responsibly.
    
### Storing your data

- You'll want to create a data volume! You'll be able to save your work here,
  and if you shut down your server, you'll be able to just remount your old data
  by entering the name of your old disk. **It is important that you remember the
  volume's name.**

![Create a Data Volume](../images/kubeflow_volumes.png)
 
!!! danger "Remember the volume name"
    At the moment we don't have an interface for searching for old volumes. This
    will hopefully be added soon, but for now you need to remember the name of
    your old volume if you want to remount it. (There is a way for engineering
    to find your old disk, but it takes time.)
 
 
### And... Create!!!

- If you're satisfied with the settings, you can now create the server! It may
  take a few minutes to spin up depending on the resources you asked for. (GPUs
  take longer.)
 
!!! success "Your server is running"
    If all goes well, your server should be running!!! You will now have the
    option to connect, and [try out Jupyter!](/1-Experiments/Jupyter)
 
 

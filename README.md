# My praises to Google
For already a while <a href='https://colab.research.google.com/'> Google Colab </a> is providing free Tesla K80 GPU to those who are interested in ~mining~ deep learning. Again, GPU for **free**, yay! 

# Сonnecting to your google drive from Colab 

Though Jupyter Notebooks are now familiar to most of the people programming in Python, the workflow on Colab may be unfamiliar to those who are not used to work on virtual machines. 

#### _One cannot just start working in Colab Notebook as they used to be working on local machine._

My first mistake was to assume that if the notebook is being created on my google drive I can access the files on the drive in the same manner as I do when working locally:

``` 
# List files from current working directory
!ls
```

See? Brand new and clean current working directory. With Google Colab you get a clear virtual machine for 12 hours and it has 
nothing to do with neither your local files nor google drive where notebook is being saved.


Moreover, google drive does not have the hierarchical structure of folders by default. From https://developers.google.com/drive/v3/web/about-files :

> Each file is identified by a unique opaque ID. File IDs are stable throughout the life of the file, even if the file name changes.
>
> Files in Drive can not be directly addressed by their path. Search expressions are used to locate files by name, type, content, parent container, owner, or other metadata.

If you want to have old-fashioned way of working with folders on the drive (e.g. to access data or to save models/submission), 
do the following routine each time you start a notebook on google colab (taken from https://www.kaggle.com/getting-started/47096#273889): 

```
# Install a Drive FUSE wrapper.
# https://github.com/astrada/google-drive-ocamlfuse
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse

# Generate auth tokens for Colab
from google.colab import auth
auth.authenticate_user()


# Generate credentialss for the Drive FUSE library.
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}

# Create a directory and mount Google Drive using that directory.
!mkdir -p drive
!google-drive-ocamlfuse drive

print('Files in Drive:')
!ls drive/
```
Newly created drive folder is now lying besides other folders that you might create during this session on this virtual machine. After 12 hours it will all ~become a pumpkin~ be deleted. 

# Working with folders 

### New project
Now you can start creating folders for the project if it's a new one, e.g.:
```
# Create directories for the new project
!mkdir -p drive/kaggle/talkingdata-adtracking-fraud-detection

!mkdir -p drive/kaggle/talkingdata-adtracking-fraud-detection/input/train
!mkdir -p drive/kaggle/talkingdata-adtracking-fraud-detection/input/test
!mkdir -p drive/kaggle/talkingdata-adtracking-fraud-detection/input/valid
```

### Existing project
If you already have an existing project on github you can clone it to Colab (here you also need to decide if you want to clone it just to your VM or you want it to be on your drive as well)

```
# Clone to the folder on google drive to have it after 12 hours
%cd drive/kaggle 
!git clone https://github.com/wxs/keras-mnist-tutorial.git 
```


To be continued...

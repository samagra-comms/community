# Requirements:

### Your machine should have yarn or npm.

#### Note: Preferable versions

``` 
Version:
    node 12
    npm 6 
    ng cli 9.1.9
```

### Check the node and npm version by running the following commands.

``` 
    node -v
    npm -v
    ng --version
```

### Installation Steps for project:

#### 1. Fork it
   You can get your own fork/copy of Frontend  by using the Fork button.


#### 2. Clone it
   You need to clone (download) it to a local machine using
   git clone https://github.com/samagra-comms/uci-admin.git


  Once you have cloned the uci-admin repository in GitHub, move to that folder first using the change directory command.

  This will change directory to a folder
```
 cd uci-admin
```
Move to this folder for all other commands.

#### 3. Set it up
   Run the following commands to see that your local copy has a reference to your forked remote repository in GitHub
```
git remote -v
```

By running the above command, you can see that the local copy has a reference to the forked remote repository in GitHub.

```
origin    https://github.com/samagra-comms/uci-admin.git (fetch)
origin    https://github.com/samagra-comms/uci-admin.git (push)
```

#### 4 Environment setup

The Environment is being used from the below file in the project root folder.
```
   .env
```
this file contains following variables:
- NG_APP_url: API base url
- NG_APP_blobUrl:  Sample file base URL
- NG_APP_botPhoneNumber: Bot phone whatsapp phone number 


#### 5. Run it

#### Install dependencies

```
npm install
```

#### 6. Build 

```
ng build --prod
```
Build @dist

#### Run frontend application in dev environment
``` 
ng serve
```

Browse - http://localhost:4000

Run standalone module in dev environment



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

### Node Installation via nvm
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

source ~/.bashrc

nvm ls-remote

after that choose the node version v12.22.12

nvm install v12.22.12

nvm use v12.22.12

nvm alias default v12.22.12

nvm use default
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



#### 4. Run it

#### Install dependencies

```
npm install
```

#### 5. Build 

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



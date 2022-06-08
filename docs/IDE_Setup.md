# Setting up IDE
## 1. Overview 
In this doc we'll configure an IDE (Eclipse/IntelliJ) for setting up development enviorment for UCI.

## 2. Setting up IntelliJ

### 2.1 Importing Projects :

* <b>Import new project :</b>  
    ```
    File -> New -> Project from existing source  
    ```
    Then open project as <b>maven project</b> in IntelliJ.

* For importing multiple projects in intelliJ click on <b>maven</b> (in right toolbar) and click on <b>+</b> for opening multiple projects.  
![add multiple projects](../media/add_multiple_projects.png)

* After importing all the projects in your IntelliJ, now reload all maven project for first time setup.
![reload projects](../media/reload_all_maven_projects.png)

### 2.2 setting configurations

For setting configuration for any project follow below steps :

* Goto edit configuration. 
![edit config](../media/edit_configuration.png)

* Add new configuration of type Application.
![new conf type application](../media/type_application.png)

* Now give name to config, select module, select JRE, give path of main class of that module, select working directory as shown in picture. 
![define properties](../media/add_name_and_module.png)

* For handling enviorment variables, click on edit enviorment variables (if this colum not shown by default, enable it from <b>modify-options</b>).
![enviorment variables](../media/env_variable.png)

* Make these configuration for following projects :  
inbound  
orchestrator  
transformer  
outbound  

### 2.3 Build and Run :

now we can build and Run the projects using below steps :

* Required Plugins to build the project :  
    maven  
    docker  
    lombok 

* Now simply select configuration and click on Run(Shift+F10), to run the project. 

## 3. Setting up Eclipse 

### 3.1 Importing Projects :

* <b>Import Projects into Eclipse :</b>
```
File 
    -> Import 
        -> Projects From Git 
            -> Existing Local Repository 
                -> Select You project
                    -> Finish
```
Import all the project like this.

* After Importing, Update all project for first time setup.
```
Project
    -> Update Maven Project
        -> Select All Projects
            -> Update
```  

![update all](../media/sts_update_all.png)

### 3.2 setting configurations

For setting configuration for any project follow below steps :

* Make new Configuration for spring-boot-app
```
Run
    -> Run Configurations
        -> Spring Boot App
```

* Now give name to config, select project, select main class of project and click Apply.  
![config properties](../media/sts_name_config.png)

* Now for handling Enviorment Variables click on Enviorment in config window.  
Here we can put enviorment variable's value.
![Enviorment Variable](../media/sts_env_variable.png)

* Make these configuration for following projects :  
inbound  
orchestrator  
transformer  
outbound

### 3.3 Build and Run :

To build and Run in Eclipse, Simply Run the project as Spring Boot App.
```
Run
    -> Run As
        -> Spring Boot App
```
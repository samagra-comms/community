# Setting up IDE

### 1. Overview

In this doc we'll configure an IDE (Eclipse/IntelliJ) for setting up development enviorment for UCI.

### 2. Setting up IntelliJ

#### 2.1 Importing Projects :

*   **Import new project :**

    ```
        File -> New -> Project from existing source  
    ```

    Then open project as **maven project** in IntelliJ.
* For importing multiple projects in intelliJ click on **maven** (in right toolbar) and click on **+** for opening multiple projects.

![add multiple projects](../../../media/add\_multiple\_projects.png)

* After importing all the projects in your IntelliJ, now reload all maven project for first time setup.

![reload projects](../../../media/reload\_all\_maven\_projects.png)

#### 2.2 Setting configurations

For setting configuration for any project follow below steps :

* Goto edit configuration.

![edit config](../../../media/edit\_configuration.png)

* Add new configuration of type Application.

![new conf type application](../../../media/type\_application.png)

* Now give name to config, select module, select JRE, give path of main class of that module, select working directory as shown in picture.

![define properties](../../../media/add\_name\_and\_module.png)

* For handling enviorment variables, click on edit enviorment variables (if this colum not shown by default, enable it from **modify-options**).

![enviorment variables](../../../media/env\_variable.png)

* Make these configuration for following projects :\
  inbound\
  orchestrator\
  transformer\
  outbound

#### 2.3 Build and Run :

Now we can build and Run the projects using below steps :

* Required Plugins to build the project :\
  maven\
  docker\
  lombok
* Now simply select configuration and click on Run(Shift+F10), to run the project.

### 3. Setting up Eclipse

#### 3.1 Importing Projects :

* **Import Projects into Eclipse :**

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

![update all](../../../media/sts\_update\_all.png)

#### 3.2 setting configurations

For setting configuration for any project follow below steps :

* Make new Configuration for spring-boot-app

```
Run
    -> Run Configurations
        -> Spring Boot App
```

* Now give name to config, select project, select main class of project and click Apply.

![](../../../media/sts\_name\_config.png)

![](../../../media/sts\_name\_config.png)

* Now for handling Enviorment Variables click on Enviorment in config window.\
  Here we can put enviorment variable's value.

![](../../../media/sts\_env\_variable.png)

* Make these configuration for following projects :\
  inbound\
  orchestrator\
  transformer\
  outbound

#### 3.3 Build and Run :

To build and Run in Eclipse, Simply Run the project as Spring Boot App.

```
Run
    -> Run As
        -> Spring Boot App
```

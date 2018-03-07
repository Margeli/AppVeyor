“I am [Carles Margelí](https://www.linkedin.com/in/carles-margel%C3%AD-549ab415a/), student of the [Bachelor’s Degree in Video Games by UPC at CITM](https://www.citm.upc.edu/cat/). This content is generated for the second year’s subject Project 2, under supervision of lecturer [Ricard Pillosu](https://www.linkedin.com/in/ricardpillosu/).”

In this article we will talk about how to avoid making builts manually in GitHub everytime a Release is made to be tested.
So, our objective, is to have an automated process that every time a commit in our code is done, a built of it is published in our repository in GitHub, including all the needed files.
To achieve it, we will be using an external application, AppVeyor. 
Following this guide step by step we will understand how AppVeyor works and how to configure it to obtain our desired results. 

I recommend this guide of [How AppVeyor works](https://www.appveyor.com/docs/enterprise/how-to/how-appveyor-works/) to understand the internal processes that it does. Even it is not necessary to understand the process we will make in this article.

# Let's start with the Tutorial:

## Starting with AppVeyor
   ● To begin with, we will need to create an account on AppVeyor using our GitHub account.

   ● Then, we will need to authorize it. If the GitHub repository to apply it is from an organization, it will require the authorization of the organization's owner. So it’s recommended to be done by the owner himself.
  
   <img src="WebPageAssets/captura1.png" alt="hi" class="inline">

   ● Once we have synchronized both applications, we can go on with AppVeyor creating a new project and selecting the GitHub repository which we want to have automated builds. 

   ● Now we have our project in AppVeyor, by default every time we make a commit, it will try make a built, but it probably fail due to the app configuration is not the correct. So the next step is how to configure it. 
 
## Configurating AppVeyor

First of all you need to know that there are two ways to configure an AppVeyor project. 
The first one it is found in the project itself, in the Settings section; there, there is an interface to help you configure the whole project. 

 <img src="WebPageAssets/captura2.png" alt="hi" class="inline">
 
 The second one is creating in your Github repository a YAML file named _appveyor.yml_ where is found the same configuration but in YAML format.
 
 <img src="WebPageAssets/captura3.png" alt="hi" class="inline">
 
 I highly recommend first of all configure your project from AppVeyor project, in the settings, and then export this changes into an _appveyor.yml_ and added into your GitHub repository. So you can change any setting from the repository. 

It’s needed to remark that **AppVeyor will give preference to the YAML file before the project settings**. So be careful on that. 


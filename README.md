“I am [Carles Margelí](https://www.linkedin.com/in/carles-margel%C3%AD-549ab415a/), student of the [Bachelor’s Degree in Video Games by UPC at CITM](https://www.citm.upc.edu/cat/). This content is generated for the second year’s subject Project 2, under supervision of lecturer [Ricard Pillosu](https://www.linkedin.com/in/ricardpillosu/).”

## Introducing AppVeyor

In the development of video games, every time a feature is added to the game in progress, it has to be tested to look for bugs or design issues, to do it correctly, the developers should upload a release with the new feature added to GitHub, and then the QA of the team will check for these errors. So in order to save time and not to make every time a built for the release manually, it’s recommended the usage of the free application [AppVeyor](https://www.appveyor.com/). We will focus on how to use AppVeyor, but there are other similar applications that share the same results, like [Jenkins](https://jenkins.io/) or [Travis CI](https://travis-ci.com/).

AppVeyor, every time a commit is done to the code, it automatically uploads the build with all the needed artifacts to the Release page of GitHub giving you feedback of how the build has been done. 

So, in this article we will talk about how to configure AppVeyor to do automatically builds to GitHub. Following this guide step by step we will understand how AppVeyor works and how to configure it to obtain our desired results. 

I recommend this guide of [How AppVeyor works](https://www.appveyor.com/docs/enterprise/how-to/how-appveyor-works/) to understand the internal processes that it does. Even it is not necessary to understand the process we will make in this article.

# Let's start with the Tutorial:


## Starting with AppVeyor
   ● To begin with, we will need to create an account on AppVeyor using our GitHub account.

   ● Then, we will need to authorize it. If the GitHub repository to apply it is from an organization, it will require the authorization of the organization's owner. So it’s recommended to be done by the owner himself.
  
   <img src="WebPageAssets/captura1.png" alt="hi" class="inline">

   ● Once we have synchronized both applications, we can go on with AppVeyor creating a new project and selecting the GitHub repository which we want to have automated builds. 

   ● Now we have our project in AppVeyor, by default every time we make a commit, it will try to make a built, but it probably fails due to the app configuration is not the correct. So the next step is how to configure it. 
 
 
## Configuring AppVeyor

First of all you need to know that there are two ways to configure an AppVeyor project. 
The first one it is found in the project itself, in the _Settings_ section; there, there is an interface to help you configure the whole project. 

 <img src="WebPageAssets/captura2.png" alt="hi" class="inline">
 
 The second one is creating in your Github repository a YAML file named _appveyor.yml_ where is found the same configuration but in YAML format.
 
 <img src="WebPageAssets/captura3.png" alt="hi" class="inline">
 
 I highly recommend first of all configure your project from AppVeyor project, in the _Settings_, and then export this changes into an _appveyor.yml_ and added into your GitHub repository. So you can change any setting from the repository. 

It’s needed to remark that **AppVeyor will give preference to the YAML file before the project settings**. So be careful on that. 

[Here](https://www.appveyor.com/docs/build-configuration/) you can check the full documentation of all the possible settings that the project has.

But in this article we will focus on the basic settings to complete our objective, that is automated builds.
The project settings is divided in different sections, the main one is _General_. There, the most relevant option is that you can configure the _Build version format_, that will increase every time a built is done (regardless of if it fails). Another useful setting is that you can select from which branch you want to make the built every time a commit is done, in _Default branch_ and _Branches to build_.

<img src="WebPageAssets/captura4.png" alt="hi" class="inline">

The next important setting is found in _Environment_ where you have to select which Visual Studio version are you using.

<img src="WebPageAssets/captura5.png" alt="hi" class="inline">

Another relevant option is that one that uploads the build done to our GitHub releases page. 
It is found in _Deployment_  and is need to change the deployment provider to _GitHub Releases_. It is recommended to add a Release description and mark the _Draft Release_ to avoid having all the releases you made there. But before all of that is needed an authentication from GitHub to let AppVeyor modify our repository. It is done through a **_GitHub authentication token_**. 

<img src="WebPageAssets/captura6.png" alt="hi" class="inline">

We will make a parenthesis to show how to get it:

### How to get your GitHub authentication token

First of all, it’s important to say that an authentication token is like a password, so manage them like that. The difference is that it is used for scripts or commands, and in addition you can revoke them and generate lots of them. 
So, to generate one of them you need to go [Here](https://github.com/settings/tokens) or manually going to your GitHub, and go to _Settings_ (the general settings, not the repository ones). There is a section _Developer Settings_ with a subsection _Personal Access Tokens_.

<img src="WebPageAssets/captura7.png" alt="hi" class="inline">

There you need to _Generate a new token_ and just select the scope _public_repo_  and then _Generate_ it.

<img src="WebPageAssets/captura8.png" alt="hi" class="inline">

Once done the token has to be copied to [Here](https://ci.appveyor.com/tools/encrypt) to encrypt it, the result is an encrypted token that has to be copied to the _GitHub authentication token_ in the _Deployment_ setting that we were talking before.

## Back to AppVeyor

At this point AppVeyor is capable to access to the Release GitHub page. 

So our objective is to make AppVeyor do automated builds from our GitHub repository, but we need to remind which items a build should have:
- A README.md file
- A folder with all the Assets of the game and the libraries .dll
- The executable of the game .exe

It is recommended putting together in a folder the ReadMe, the assets and the libraries to make the process easily. In all the explanation we will refer to this folder as _\Game_.

So we need to upload this _Game_ folder together with the executable of the game, that will be given by AppVeyor once it has made the Release. To do it we need to go again to the project _Settings_. 
Firstly we need to go to _Build_ section and fill the _Configuration_ option with Debug and Release.  We put both, to check that there’s no problem compiling the code in Debug nor Release mode. Then, as we said before, we need to get the executable given after the AppVeyor does the Release to our code. So in _Before packaging script_ we need to insert a script in PS (_PowerShell_) language which will copy this executable to the _Game_ folder, to have it all together. 

The script is the following:
```ruby
Copy-Item C:\projects\(your_project_name)\$env:CONFIGURATION\(your_solution_name).exe C:\projects\(your_project_name)\Game\.
```
_"Where the directory to copy is the solution of your Debug/Release, and the directory to paste it is your Game carpet"_

_"($env:CONFIGURATION): refers to Debug or Release folder"_

Below we have an example 

<img src="WebPageAssets/captura9.png" alt="hi" class="inline">

So now, after it is done the Release to our code, the folder _Game_ will contain all that a correct built needs. So the last step is to upload this folder to our GitHub Release page.
It is configured in the section _Artifacts_ where we need to put the path to the folder _Game_, with a name for the release and selected the _Web Deploy Package_ type.

<img src="WebPageAssets/captura10.png" alt="hi" class="inline">

If the steps are followed correctly the build should be uploaded to Release page as a draft every time a commit is done in the project. I recommend, as we said before, to export all the configuration to YAML format and upload to the repository to allow the modification directly from GitHub.

<img src="WebPageAssets/captura11.png" alt="hi" class="inline">

Another useful feature that AppVeyor provides is _Notification_, every time a built is done it will notificate it through the channels that you prefer, the most common are Email, Slack.. There, you can select when it has to notificates you. Whether the built has been upload successfully or it failed, or both. 

<img src="WebPageAssets/captura12.png" alt="hi" class="inline">

### Links to more documentation
[Official AppVeyor Tutorial](https://www.appveyor.com/docs/)

[Documentation of the configuration of AppVeyor](https://www.appveyor.com/docs/build-configuration/)

[How AppVeyor works](https://www.appveyor.com/docs/enterprise/how-to/how-appveyor-works/)


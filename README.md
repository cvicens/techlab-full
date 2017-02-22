![](./images/Header-lab-full.png)

<!-- toc -->

- [Introduction](#introduction)
- [Pre-requisites](#pre-requisites)
- [Log in to RHMAP](#log-in-to-rhmap)
- [Introduction](#introduction-1)
- [Let’s start by creating an MBaaS Service.](#lets-start-by-creating-an-mbaas-service)
- [Create an empty project and import assets](#create-an-empty-project-and-import-assets)
  * [Let’s import the dashboard Web App](#lets-import-the-dashboard-web-app)
  * [Add a backend Service to our project](#add-a-backend-service-to-our-project)
  * [Create a Drag & Drop App to submit form data](#create-a-drag--drop-app-to-submit-form-data)

<!-- tocstop -->

# Introduction

This guide should help you creating a mobile application to provide feedback (for an event for example) and see results in real time in a dashboard (web application).

As you run through the steps you’ll create a mobile app, a web application and an MBaaS service to persist the form submissions to a backend (incidentally MongoDB, but it could be any other system of record).

# Pre-requisites

In order to run this guide you need a Red Hat Mobile environment. We’re going to assume you’re going to run this guide in this environment [https://techlab.us.events.redhatmobile.com](https://techlab.us.events.redhatmobile.com)

You will need a GIT client installed locally.

# Log in to RHMAP

You should have been provided with a user to access the platform. If not please request your credentials to the Redhatter in charge of the session.

Log in with your credentials [https://techlab.us.events.redhatmobile.com](https://techlab.us.events.redhatmobile.com)
![](./images/001.png)

# Introduction

We need to deploy a project containing the following apps:

* **Tech Lab Admin App:** this is the web app where we will see the data captured using forms as a list and pie charts

* **Tech Lab Client App:** Forms App used to capture feedback as a form submission

* **Tech Lab Cloud App:** out of the box cloud app including a service to deal with the data coming from the Forms Submissions Backend Service and a listener to capture submissions and send them over to the Forms Submissions Backend Service

And an MBaaS Service:

* **Forms Submissions Backend Service:** this MBaaS services plays also the role of backend because it represents the system of record in this example by using MongoDB

# Let’s start by creating an MBaaS Service.

Go to ‘Services and APIs’ and click on the new ‘Provision new MBaaS Service’ button. Then choose the ‘New mBaaS Service’ template and give it the following name:  Tech Lab Backend Service.
![](./images/002.png)
![](./images/003.png)
Click Finish.
![](./images/004.png)

Now you should see the home screen where you’ll confirm that the service is stop. In fact it hasn’t been deployed yet.
![](./images/005.png)

Copy the GIT url and go to your terminal screen.
![](./images/006.png)

Create a directory ‘tech-lab’ could be a good option. We’re going to create several assets there, starting by cloning the repo where there are the source files of the MBaaS service we have just created. Change to that directory.

```
$ mkdir tech-lab
$ cd tech-lab
```

Now lets clone the repo and inspect the files.

```
$ git clone git@git.tom.redhatmobile.com:techlab/Tech-Lab-Backend-Service-Tech-Lab-Backend-Service.git
Cloning into 'Tech-Lab-Backend-Service-Tech-Lab-Backend-Service'...
...
X11 forwarding request failed on channel 0
remote: Counting objects: 22, done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 22 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (22/22), 8.40 KiB | 0 bytes/s, done.
Checking connectivity... done.
$ cd Tech-Lab-Backend-Service-Tech-Lab-Backend-Service/
$ ls
Grunt-ReadMe.md	Gruntfile.js	LICENSE		README.md	application.js	lib		package.json	public		test
```

Now let’s delete all the files and the results with git status

```
$ rm -rf *
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    Grunt-ReadMe.md
...
	deleted:    test/server.js
	deleted:    test/unit/test-hello.js
	deleted:    test/unit/test_application.js

no changes added to commit (use "git add" and/or "git commit -a")
```

Commit changes

```
$ git commit -a -m "empty" ; git push
```

Now let’s point our local repo to another remote repo. This is the url to the needed remote repo: https://github.com/cvicens/Form-Submissions-Backend-Service.git
```
$ git remote add github https://github.com/cvicens/Form-Submissions-Backend-Service.git
$ git pull github master
warning: no common commits
remote: Counting objects: 36, done.
remote: Total 36 (delta 0), reused 0 (delta 0), pack-reused 36
Unpacking objects: 100% (36/36), done.
From https://github.com/cvicens/Form-Submissions-Backend-Service
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> github/master
Merge made by the 'recursive' strategy.
 Grunt-ReadMe.md               |  52 ++++++++++++++++++++++++++++++++++++++++++
 Gruntfile.js                  | 164 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 LICENSE                       |  23 +++++++++++++++++++
...
 test/unit/test_application.js | 122 ++++++++++++++++++++++++++++++++++++++++++++++++++++
 15 files changed, 1032 insertions(+)
 create mode 100644 Grunt-ReadMe.md
 create mode 100644 Gruntfile.js
 create mode 100644 LICENSE
...
 create mode 100644 test/accept/test-hello.js
 create mode 100644 test/server.js
 create mode 100644 test/unit/test-hello.js
 create mode 100644 test/unit/test_application.js
$ ls
Grunt-ReadMe.md	Gruntfile.js	LICENSE		README.md	application.js	lib		package.json	public		test</td>
  </tr>
  <tr>
    <td>Finally let’s remove the pointer to the github remote repo and push the changes to the original remote repo, sitting in RHMAP.</td>
  </tr>
  <tr>
    <td>$ git remote remove github
$ git status
On branch master
Your branch is ahead of 'origin/master' by 5 commits.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean
$ git push
warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the traditional behavior, use:
...
X11 forwarding request failed on channel 0
Counting objects: 34, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (31/31), done.
Writing objects: 100% (34/34), 11.18 KiB | 0 bytes/s, done.
Total 34 (delta 7), reused 15 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Backend-Service-Tech-Lab-Backend-Service.git
   4c0fbb0..0370aa8  master -> master
```

Now that the needed sources are in RHMAP we should deploy the app. Click on the ‘Deploy’ (cloud) icon and then on the ‘Deploy.
![](./images/007.png)

Do the same for the other environments by changing the environment on the dropdown list at the top right corner.</td>



# Create an empty project and import assets

Let’s go to ‘Projects’ and click on ‘New Project’
![](./images/008.png)

Select ‘Bare Project’, this is a empty shell. Later we’ll add the client and admin apps and also a Cloud App. Give the project a name, for instance: Tech Lab
![](./images/009a.png)
![](./images/009b.png)

Now let’s create a Cloud App. To do so, click on the plus ‘+’ sign in the Cloud Apps area.
![](./images/010a.png)
![](./images/010b.png)

Select ‘Cloud App’, not the clustered app version.
![](./images/011a.png)

Give the Cloud App a name (give it a descriptive name), such as: Tech Lab Cloud App
![](./images/011b.png)

Copy the git repo url.
![](./images/012a.png)
![](./images/012b.png)

Clone the repo.

```
$ git clone git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Cloud-App.git
```

Change dir to the new folder: Tech-Lab-Tech-Lab-Cloud-App

```
$ cd Tech-Lab-Tech-Lab-Cloud-App/
```

Remove everything but the .git* files

```
$ rm -rf .jshintrc .travis.yml *
```
  
Add a new remote pointing to: https://github.com/cvicens/Tech-Forum-Cloud-App.git

```
$ git remote add github https://github.com/cvicens/Tech-Forum-Cloud-App.git
```

Let’s pull the new sources from the added remote git. If an editor opens (probably vi in Mac and Linux) just save and close.

```
$ git pull github master
warning: no common commits
remote: Counting objects: 64, done.
remote: Total 64 (delta 0), reused 0 (delta 0), pack-reused 64
Unpacking objects: 100% (64/64), done.
From https://github.com/cvicens/Tech-Forum-Cloud-App
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> github/master
Merge made by the 'recursive' strategy.
 .jshintrc                      |  31 ++++++++++++++++++
 .travis.yml                    |  13 ++++++++
 Grunt-ReadMe.md                |  52 +++++++++++++++++++++++++++++
 Gruntfile.js                   | 164 +++++++++++++++++++++++++++++++++++++++++++++++
 LICENSE                        |  23 +++++++++++++
 README.md                      |  46 ++++++++++++++++++++++++++
...
 public/index.html              |   1 +
 test/accept/test-hello.js      |  49 ++++++++++++++++++++++++++++
 test/server.js                 |  42 ++++++++++++++++++++++++
 test/unit/test-hello.js        | 149 +++++++++++++++++++++++++++++++++++++++++++++++++++
 test/unit/test_application.js  | 122 +++++++++++++++++++++++++++++++++++++++
 21 files changed, 1586 insertions(+)
...
 create mode 100644 test/unit/test_application.js
```
 
Let’s remove the remote git repo reference and commit changes to the RHMAP repo.

``` 
$ git remote remove github
$ git commit -a -m "imported" ; git push
On branch master
Your branch is ahead of 'origin/master' by 10 commits.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean
warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the traditional behavior, use:
...
X11 forwarding request failed on channel 0
Counting objects: 64, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (61/61), done.
Writing objects: 100% (64/64), 18.72 KiB | 0 bytes/s, done.
Total 64 (delta 29), reused 15 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Cloud-App.git
   442c1f0..9fed8eb  master -> master
$ git status

On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

Now let’s go back to RHMAP Studio and deploy our Cloud App. Click on the cloud icon on the left, as in the image.
![](./images/013.png)

Confirm that the environment is ‘Team A’. Now let’s deploy the App by clicking on the ‘Deploy App’ button. If everything works as expected you should see the environment selected turn green.
![](./images/014.png)

Do the same for the other environments by changing the environment on the dropdown list at the top right corner.

Let’s test if the App actually works. To do so, click on the ‘Docs’ icon and then to the ‘Try it’ Button in the GET /submission line. Then click on ‘Submit’
![](./images/015.png)


## Let’s import the dashboard Web App

Let’s open the project (‘Tech Lab’ in my case) and click on the plus sign in the Client Apps area and click on ‘Create New App’
![](./images/016.png)

Start typing ‘web’ in the search field and choose the ‘Blank Web App’ template. Choose a descriptive name, for instance: Tech Lab Admin App. Click ‘Create Client App’
![](./images/017a.png)
![](./images/017b.png)

Now from the home page of the App, let’s copy the url of the git repo.
![](./images/018a.png)
![](./images/018b.png)

Let’s go to the terminal screen where we’ll clone the repo and clone the repo, then change dir to the repo folder

```
$ git clone git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git 
Cloning into 'Tech-Lab-Tech-Lab-Admin-App'...
...
X11 forwarding request failed on channel 0
remote: Counting objects: 16, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 16 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (16/16), 278.60 KiB | 134.00 KiB/s, done.
Checking connectivity... done.
$ cd Tech-Lab-Tech-Lab-Admin-App/
```

Delete everything and commit.

```
$ rm -rf *
$ git commit -a -m "empty" ; git push
[master 37a7812] empty
 Committer: Carlos Vicens <cvicensa@cvicensa-mbp.local>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 11 files changed, 14063 deletions(-)
 delete mode 100644 Gruntfile.js
...
 delete mode 100644 screenshots/advanced_webappproject_container.png
 delete mode 100644 screenshots/advanced_webappproject_nocontainer.png
warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'. To squelch this message

...

X11 forwarding request failed on channel 0
Counting objects: 2, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (1/1), done.
Writing objects: 100% (2/2), 235 bytes | 0 bytes/s, done.
Total 2 (delta 0), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
   6caafb5..37a7812  master -> master
```

Add a remote git reference pointing to: https://github.com/cvicens/Tech-Forum-Admin-App.git and pull source files from there.

```
$ git remote add github https://github.com/cvicens/Tech-Forum-Admin-App.git
$ git pull github master
warning: no common commits
remote: Counting objects: 722, done.
remote: Total 722 (delta 0), reused 0 (delta 0), pack-reused 722
Receiving objects: 100% (722/722), 2.97 MiB | 159.00 KiB/s, done.
Resolving deltas: 100% (172/172), done.
From https://github.com/cvicens/Tech-Forum-Admin-App
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> github/master
Merge made by the 'recursive' strategy.
 .jshintrc                                                                     |    27 +
  ...
 www/views/list.html                                                           |    78 +
 www/views/submissions.html                                                    |    73 +
 509 files changed, 199024 insertions(+)
 create mode 100644 .jshintrc
 ...
 create mode 100644 www/views/list.html
 create mode 100644 www/views/submissions.html
 ```
 
 Remove the remote reference to the github repo and push changes to the original repo.

```
$ git remote remove github
$ git remote
origin
$ git push
warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the traditional behavior, use:
...
X11 forwarding request failed on channel 0
Counting objects: 722, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (528/528), done.
Writing objects: 100% (722/722), 2.97 MiB | 335.00 KiB/s, done.
Total 722 (delta 173), reused 720 (delta 172)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
   37a7812..5f2997d  master -> master<
```

Now, let’s go back RHMAP Studio and open Tech Lab project and then Tech Lab Admin App. Once there go to ‘Connections’ option in the App upper toolbar. You’ll see just one connection (it should be pointing to the first environment), click on the ‘Configure’ button on the right and copy the JSON content.
![](./images/019a.png)
![](./images/019b.png)
![](./images/019c.png)

Click on the ‘Editor’ icon on the left, open the file www/fhconfig.json and substitute its contents with the contents in the clipboard (paste).
![](./images/020.png)

Now it’s time to re-deploy the App. Click on the ‘cloud’ icon on the to enter the deployment interface. Once there, click on the ‘Deploy App’ button.
![](./images/021a.png)
![](./images/021b.png)

Go back to the App home page and click on the ‘Current Host’ url. This will open a new tab or a window in your browser pointing to the web app running in the environment selected in the dropdown list. In the following screen the environment selected is Team A.
![](./images/022.png)

Below the web App running.
![](./images/023.png)

Now we have to create a branch for each the environments so that each of the connection file ‘fhconfig.json’ points to the right environment.

```
$ pwd
/Users/cvicensa/Projects/rhmap/tech-lab
$ cd Tech-Lab-Tech-Lab-Admin-App/
$ git branch
* master
$ git checkout -b team_a ; git push origin team_a
Switched to a new branch 'team_a'
...
Total 729 (delta 174), reused 724 (delta 172)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
 * [new branch]      team_a -> team_a
$ git checkout -b team_b ; git push origin team_b
Switched to a new branch 'team_b'
...
Total 0 (delta 0), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
 * [new branch]      team_b -> team_b
$ git checkout -b team_c ; git push origin team_c
Switched to a new branch 'team_c'
...
Total 0 (delta 0), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
 * [new branch]      team_c -> team_c
$ git checkout -b team_d ; git push origin team_d
Switched to a new branch 'team_d'
...
Total 0 (delta 0), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
 * [new branch]      team_d -> team_d
$ git branch
  master
  team_a
  team_b
  team_c
* team_d
```

Now, we will create a new directory to clone each of the branches at. Use the same git repo url as before.

```
$ pwd
/Users/cvicensa/Projects/rhmap/tech-lab
$ mkdir admin-app-branches
$ cd admin-app-branches/
$ pwd
/Users/cvicensa/Projects/rhmap/tech-lab/admin-app-branches
$ git clone -b team_a --single-branch git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
Cloning into 'Tech-Lab-Tech-Lab-Admin-App'...
...
Checking connectivity... done.
$ mv Tech-Lab-Tech-Lab-Admin-App/ Tech-Lab-Tech-Lab-Admin-App_team_a/
$ git clone -b team_b --single-branch git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
Cloning into 'Tech-Lab-Tech-Lab-Admin-App'...
...
Checking connectivity... done.
$ mv Tech-Lab-Tech-Lab-Admin-App/ Tech-Lab-Tech-Lab-Admin-App_team_b/
$ git clone -b team_c --single-branch git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
Cloning into 'Tech-Lab-Tech-Lab-Admin-App'...
...
Checking connectivity... done.
$ mv Tech-Lab-Tech-Lab-Admin-App/ Tech-Lab-Tech-Lab-Admin-App_team_c/
$ git clone -b team_d --single-branch git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
Cloning into 'Tech-Lab-Tech-Lab-Admin-App'...
...
Checking connectivity... done.
$ mv Tech-Lab-Tech-Lab-Admin-App/ Tech-Lab-Tech-Lab-Admin-App_team_d/
```

Now we are going to create a connection for each branch pointing to its corresponding environment (all for the web Admin App). As you can see in the next picture there are only two connections, we’re going to create as many as environments (we could skip Team A connection, but we’re going to do it for the sake of uniformity)
![](./images/024.png)

Let’s create our new connections. You’ll see in the next pictures that once you click on the create new connection button you have to select which Cloud App the client application (Admin App in this case) should be connected to and also the client app the connection is for.
![](./images/025a.png)
![](./images/025b.png)
 
In the end you should get to this situation.
![](./images/026.png)

We’re going to need the connection information for each of the branches we have just created. To see/edit the connection click ‘Configure’. Next images shows the connection data.
![](./images/027.png)

Now it’s time to update www/fhconfig.json file for each branch to use the appropriate connection. In our case, as reflected in the previous image. 

* Team A ⇒ 0.0.3
* Team B ⇒ 0.0.4
* Team C ⇒ 0.0.5
* Team D ⇒ 0.0.6

```
Tech-Lab-Tech-Lab-Admin-App_team_a/www/fhconfig.json → connectiontag = 0.0.3
{
  "appid": "fxfbxbjcctghwsvati36fmck",
  "appkey": "69280f63d5984259a5c334b0e373ac04565361b5",
  "apptitle": "Tech Lab Admin App",
  "connectiontag": "0.0.3",
  "host": "https://techlab.us.events.redhatmobile.com",
  "projectid": "fxfbxbj5ja54ok3u3valoxy6"
}


Tech-Lab-Tech-Lab-Admin-App_team_b/www/fhconfig.json → connectiontag = 0.0.4
{
  "appid": "fxfbxbjcctghwsvati36fmck",
  "appkey": "69280f63d5984259a5c334b0e373ac04565361b5",
  "apptitle": "Tech Lab Admin App",
  "connectiontag": "0.0.4",
  "host": "https://techlab.us.events.redhatmobile.com",
  "projectid": "fxfbxbj5ja54ok3u3valoxy6"
}

Tech-Lab-Tech-Lab-Admin-App_team_b/www/fhconfig.json → connectiontag = 0.0.5
{
  "appid": "fxfbxbjcctghwsvati36fmck",
  "appkey": "69280f63d5984259a5c334b0e373ac04565361b5",
  "apptitle": "Tech Lab Admin App",
  "connectiontag": "0.0.5",
  "host": "https://techlab.us.events.redhatmobile.com",
  "projectid": "fxfbxbj5ja54ok3u3valoxy6"
}

Tech-Lab-Tech-Lab-Admin-App_team_b/www/fhconfig.json → connectiontag = 0.0.6
{
  "appid": "fxfbxbjcctghwsvati36fmck",
  "appkey": "69280f63d5984259a5c334b0e373ac04565361b5",
  "apptitle": "Tech Lab Admin App",
  "connectiontag": "0.0.6",
  "host": "https://techlab.us.events.redhatmobile.com",
  "projectid": "fxfbxbj5ja54ok3u3valoxy6"
}
```

Once you have change fhconfig.json files you have to commit those changes and re-deploy.

```
$ cd Tech-Lab-Tech-Lab-Admin-App_team_a
$ git status
On branch team_a
Your branch is up-to-date with 'origin/team_a'.
...
	modified:   www/fhconfig.json
...
$ git commit -a -m "connection to team a"; git push origin team_a
[team_a d253539] connection to team a
...
Total 4 (delta 3), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
   5f2997d..d253539  team_a -> team_a

$ cd ../Tech-Lab-Tech-Lab-Admin-App_team_b
$ git status
On branch team_b
Your branch is up-to-date with 'origin/team_b'.
...
	modified:   www/fhconfig.json
...
$ git commit -a -m "connection to team b"; git push origin team_b
[team_a d253539] connection to team b
...
Total 4 (delta 3), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
   5f2997d..390e6b7  team_b -> team_b
$ cd ../Tech-Lab-Tech-Lab-Admin-App_team_c
$ git status
On branch team_c
Your branch is up-to-date with 'origin/team_c'.
...
	modified:   www/fhconfig.json
...
$ git commit -a -m "connection to team c"; git push origin team_c
[team_a d253539] connection to team c
...
Total 4 (delta 3), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
   5f2997d..22dcad0  team_c -> team_c
$ cd ../Tech-Lab-Tech-Lab-Admin-App_team_d
$ git status
On branch team_d
Your branch is up-to-date with 'origin/team_d'.
...
	modified:   www/fhconfig.json
...
$ git commit -a -m "connection to team d"; git push origin team_d
[team_a d253539] connection to team d
...
Total 4 (delta 3), reused 0 (delta 0)
To git@git.tom.redhatmobile.com:techlab/Tech-Lab-Tech-Lab-Admin-App.git
   5f2997d..c7feb0b  team_d -> team_d
```

Let’s deploy each branch version to its corresponding environment.
![](./images/028a.png)
![](./images/028b.png)
![](./images/028c.png)
![](./images/028d.png)

## Add a backend Service to our project

Let’s go back to the project view and add our MBaaS Service (Tech Lab Backend Service) to it.

Open ‘Tech Lab’ project and click on the plush ‘+’ sign in the MBaaS Services area.
![](./images/029.png)

Now choose our MBaaS Service: Tech Lab Backend Service
![](./images/030.png)

Now from the project view click on the service just added. We need to copy the service ID.
![](./images/031.png)

From the MBaaS Service home page copy the service ID.
![](./images/032.png)

Now we have to create an environment variable to store the Service ID. This is going to be used from the Cloud App code to persist Form submissions by invoking our Tech Lab Backend Service. To do so:
From the project view click on the Cloud App
Click on the ‘Environment Variables’ icon on the left and then on ‘Add Variable’
![](./images/033.png)

The name of the new variable is ‘BACKEND_SERVICE_GUID’ and the value is the service ID of our Tech Lab Backend Service that we copied before. Then click on ‘Add’ and then ‘Confirm’, this action restarts the App.
![](./images/034a.png)
![](./images/034b.png)

Do the same for the other environments. Note that the name and the value of the environment variable is the same for all the environment.</td>


## Create a Drag & Drop App to submit form data

Now we have to create a client app based on forms (aka ‘Drag & Drop Apps’)

Open project Tech Lab. And click on the plus sign ‘+’ in the Client Apps area. Then click on the ‘Create New App’ button.
![](./images/035.png)

Start typing ‘form’ in the search field. Select the ‘Cordova Forms App’ template. Provide a name: Tech Lab Client App and click on ‘Create Client App’ to generate the app.
![](./images/036.png)

Now we’re going to create a Form and a Theme for the App we’ve just created. Click on ‘Drag & Drop Apps’ and then on Forms Builder.
![](./images/037.png)

Now click on ‘New Form’
![](./images/038.png)

Select ‘Feedback Form’ template.
![](./images/039.png)

Give your form a name, for example ‘Tech Lab Feedback Form v1.0’
![](./images/040.png)

Let’s do a couple of changes:

* Delete ‘Name’ field
* Add a new radio button under ‘How would you rate your experience?’
* The name of the field should be: ‘Would you repeat?’
* Radio button values: Yes, No, I don’t know
* Uncheck ‘required’ for ‘Further comments’ and ‘Today’s Date’
* Click ‘Save & Deploy’
![](./images/041.png)

After clicking ‘Save & Deploy’ you’ll get at the Deploy area. Confirm ‘Team A’ is the environment selected in the dropdown and click on ‘Deploy’
![](./images/042.png)

Do the same (deploy) in the other environments.


Now we’re going to create a Theme. Click on ‘Drag & Drop Apps’ and then on Forms Theme.
![](./images/043.png)

Now click on ‘New Theme’
![](./images/044.png)

Select Red Hat template and give your theme a name, for example: Tech Lab Theme v1.0
![](./images/045.png)

Click save and should get to the Theme Editor. As in the image below.
![](./images/046.png)

Let’s go back to the Project view for project ‘Tech Lab’ and click on ‘Forms’
![](./images/047.png)

Now we’re at the interface where we can associate a Theme and one or more Forms.
![](./images/048.png)

Select the theme and form we just created and click ‘Save’
![](./images/049.png)

Click on ‘Apps, Cloud Apps & Services’ and go to the Cloud App. Before we proceed with the tests using the Client App (Tech Lab Client App) we have to confirm which environment we’re going to test against. Please select one of the environments and take note of it. In the image below we’re selecting ‘Team A’ environment.
![](./images/050.png)

Let’s go back to the project view of ‘Tech Lab’ project and from there go to app we called ‘Tech Lab Client App’. As you can see in the image below if everything is fine you should see on the right side a simulator and our app running. You should see one button for the Form we just created and the theme (logos, colors) should match those in the theme we created and selected before.
(*) if you see an error message please check if the Cloud App is running
![](./images/051.png)

Before we try our client App we’re going to open the Admin Portal and understand its role.
Click on ‘Apps, Cloud Apps & Services’ and then on Tech Lab Admin App.
![](./images/052.png)

Now click on the link close to the label ‘Current Host’ you should see something like the next image. Note that this link is for the environment selected in the dropdown list, in this case ‘Team A’.
![](./images/053a.png)
![](./images/053b.png)

Let’s try the client app so go to ‘Apps, Cloud Apps & Services’ and then on Tech Lab Client App’. On the right side you should see the App in the simulator with a button for the form we created before. Click on that button and then fill in the form and click submit.
![](./images/054a.png)
![](./images/054b.png)

Now let’s go back to the dashboard and see the results, by default results are retrieved from the Forms Submissions database. Click on the ‘Backend’ button and you’ll see the same information, this time retrieved from the Tech Lab Backend Service which is exposed through the Cloud App. If you click on the ‘Go to list’ link (upper right corner) you’ll see the same information but in a table.

Pie chart from ‘Forms Submissions’
![](./images/055a.png)

Pie chart from ‘Forms Backend</td>
![](./images/055b.png)

Table from ‘Forms Submissions’</td>
![](./images/055c.png)

Table from ‘Backend’</td>
![](./images/055d.png)

Now let’s build the Client App (this exercise illustrates the build for iOS but you can do it for Android in the same fashion). To build the Client App, click on ‘Build’ on the left. Select the environment in the dropdown list (upper right corner). Select the platform (iOS in this case) and credential bundle, enter your password and click ‘Build’.
![](./images/056.png)

If the build is successful you’ll see a QR code you can use to download the binary.
![](./images/057.png)

If you want to build the Android binary (APK) you won’t need a credentials bundle as long as you build it for Debug. In order to install this APK you need to set up your terminal to accept APK from an unknown developer, of course in RHMAP you can manage Android credentials bundles as well as for iOS and Windows, in that case you don’t need to accept unknown sources. Next image shows this scenario.
![](./images/058a.png)
![](./images/058b.png)

Open ‘Connections’ and pay attention to the connections generated after building the binaries for iOS and Android. In the image below Android is 0.0.10 and iOS is 0.0.9
![](./images/059.png)


Now you can use the simulator and the iOS/Android app to submit data and see results in our Tech Lab Admin App.

Because we have several environments set up you can now change the client app connection from one environment to another and see results in the Admin App in those environments.
![](./images/060.png)

Let’s say we move the iOS connection from Team A to Team D
Now kill the client App ‘Tech Lab Client App’ and open it again, fill the feedback form and submit it as we did before. Open the Admin App for the environment ‘Team D’ and watch the results. 
![](./images/061.png)



# NICE (Northeastern Interactive Clustering Engine)
The Northeastern Interactive Clustering Engine (NICE) is an open source 
data analysis framework, which aims to helping researchers in different 
domains gain insight in their data by providing an interactive 
webpage-based interface with a set of clustering algorithms and the 
capability to visualize the clustering results. The framework is still 
under development.

## For Contributors:
There are two ways to contribute to this project. If you are added to the project as a collaborator, please follow the steps in "Using Branch" section. Otherwise, you can fork the project and submit pull requests; the instructions are listed in "Using Fork" section. The most important rule here is that we only use pull request to contribute and we never push directy to the master or develop branch.

### Using Branch:
1. Clone the repository: `git clone git@github.com:yiskylee/NICE.git`.
2. Create your own local feature branch: `git checkout -b your-own-feature-branch develop`
3. Make your own feature branch visible by pushing it to the remote repo (DO NOT PUSH IT TO THE DEVELOP BRANCH): `git push --set-upstream origin your-own-feature-branch`
4. Develop your own feature branch in your local repository: `git add`, `git commit`, etc..
5. After your own branch is completed, make sure to merge the latest change from the remote develop branch to your own local develop branch: 1) `git checkout develop` 2) `git pull`.
6. Now that your local develop branch is up to date, you can update your own feature branch by: 1) `git checkout your-own-feature-branch` 2) `git pull origin develop`.
7. Update your own feature branch on the remote repository by: `git push origin your-own-feature-branch`
8. Make a pull request with base being develop and compare being your-own-feature-branch
9. After the pull request is merged, your-own-feature-branch on the remote repository will be soon deleted, delete it on your local repository by: `git branch -d your-own-feature-branch`

### Using Fork:
1. Fork the repository to your own remote repository.
2. Git clone the repository: `git clone git@github.com/your_account_name/NICE.git`
3. Add this project as an upstream to your local repository by `git remote add upstream https://github.com/yiskylee/NICE.git`. You can use `git remote -v` to view the upstream.
4. Create your own local feature branch: `git checkout -b your-own-feature-branch develop`
3. Make your own feature branch visible by pushing it to your own remote repository (DO NOT PUSH IT TO THE DEVELOP BRANCH): `git push --set-upstream origin your-own-feature-branch`
4. Develop your own feature branch in your local repository: `git add`, `git commit`, etc..
5. After your own branch is completed, make sure to merge the latest change from upstream develop branch to your own origin develop branch: 1) `git checkout develop` 2) `git pull upstream develop` 3) `git push origin develop`
6. Since that you have the latest change in your own origin develop branch from upstream one, now you can update your own feature branch on the your own remote repository by: 1) `git checkout your-own-feature-branch` 2) `git pull origin develop` 3) `git push origin your-own-feature-branch`
7. Make a pull request from your own feature branch on your own remote repository on github to the develop branch of this repository.
8. After the pull request is merged, you can delete your own feature branch by 1) `git push origin --delete your-own-feature-branch` to delete the remote branch and 2) `git branch -d your-own-feature-branch` to delete your local branch.
9. More instructions on using fork can be found [here](https://help.github.com/articles/fork-a-repo/).

## Compile and Test Nice:
We use CMake tool to automatically build and test the framework. After you download the repository, you need to go to NICE/cpp and run `./configure.sh`. This is only a one time operation as it will create a build directory where all executables generated will be put into. To build the code and the tests, go to build directory and run 1) `make` 2) `make test ARGS="-V"`.

## Coding Style:
We are following [Google c++ style guide](https://google.github.io/styleguide/cppguide.html), make sure to use `google_styleguide/cpplint/cpplint.py` to check your code and make sure there are no errors. Additionally, `cpplint.py` has been integrated to Nice together with cmake, so you should be able to check your code through cmake-generated Makefile. After you run `./configure.sh` indicated in previous section, go to build directory and run `make check`.
For developers preferring IDE like Eclipse, you can also import `eclipse-cpp-google-style.xml`(Can be found from [Google c++ style guide](https://google.github.io/styleguide/cppguide.html)) into Eclipse to auto-format your code before using `cpplint.py` or `make check`.

## Documentation
We are using Doxygen to automatically generate project documents. To produce the html based documents, you should run `make doc` after you run `./configure.sh`. Make sure Doxygen is intalled on your computer. For Ubuntu users, type command `sudo apt-get install doxygen` to intall it. For more information about Doxygen, check their [official website](www.doxygen.org).
All documents will be generated under directory doc/html. Double click index.html to browse generated documents from any web browser you like(Chrome, Firefox etc.)

## Hosting the documentation on Github Pages via Submodules
We will be using Github pages in order to publically host our compiled doxygen files. 

Instructions loosley adapted from [martinhh](https://martinhh.github.io/2014/08/27/hosting-doxygen-as-github-page/). 

### Pre-Conditions
Suppose the following things are set up:

1. Your local and remote master are up-to-date and don’t have any uncommited changes.
2. We are currently located in the root directory of the the project (NICE) folder. 

For testing purposes, we will set this up through another test branch "siteSubmod". 

`$ git checkout -b siteSubmod`

First, we need to edit the .gitignore file so that we can track changes to our compiled files. While generally we do NOT track the changes of compiled files, the compiled html files MUST be pushed up to our remote repository in order to be "hosted" online. 

```
$ vim .gitignore
#Comment out the line "cpp/doc"
```
Now that we can track the changes in our cpp/doc folder, remove all of the old compiled doxygen files within cpp/doc/html. 

```    
# delete the cpp/doc/html
$ rm -rf cpp/doc/html
# add and commit:
$ git add -A
$ git commit -m "Removed contentes of cpp/doc/html (to be re-added as submodule)"
```
Then, we need to create our gh-pages branch as an orphan branch so that it has its has its own history, independent from the rest of the branches and commits. We want to delete everything in this branch and commit it as an empty repository. 

```
$ git checkout --orphan gh-pages
$ rm -rf *
$ git add .
$ git commit -m "Initialize gh-pages branch as empty directory"
$ git push origin gh-pages
```
With our gh-pages branch created, we need to change back to the siteSubmod branch. 

`$ git checkout siteSubmod`

Next, we add the submodule, placing it in the position of the Doxygen output folder:

`$ git submodule add -b gh-pages <linkToClone> cpp/doc/html`

Now, the project’s gh-pages branch should automatically be cloned into that folder.

The directory cpp/doc/html now contains an entirely separate git repo. If you were to type `$ ls -la` within the new directory, you would see this directory contains its own ".git" and ".gitignore" files. Similarly, running `$ git branch` would reveal different branches from those in the root directory of your project (in our instance, the submodule should only contain gh-pages and the default branch). 

Because we are only using the submodule for the gh-pages branch, we can delete the other branches (within this repo. 
```
# delete master branch:
$ git branch -d master
# If you have other branches that were cloned as well, remove those there. 
```
Now we can recompile our Doxygen output.
```
$ cd ../../build
$ make doc
```
With the doxygen files recompiled, we can add, commit, and push to our repo. 

```
$ cd ../doc/html
$ git add .
$ git commit -m "Initial Doxygen commit"
$ git push origin gh-pages
```

Finally, we commit the changes of the submodule to the master branch:
```
# go to the root dir of the master branch:
$ cd ../../..
## add, commit and push:
$ git add .
$ git commit -m "Set up html directory as gh-pages submodule"
$ git push origin master
```
That’s it.

Updating the Doxygen Github page
Once everything is setup, whenever you want to update the Doxygen page, just re-render your Doxygen output and repeat the last few steps described after re-rendering the Doxygen output: add, commit and push all changes to the gh-pages branch and commit the changes of the submodule to the master branch (or create some script that does that…).



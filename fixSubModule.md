## Problem, and symptoms:

You experience one or more of these symptoms
- you have code in a directory but it's not being pushed to GitHub. You just see an empty directory icon
- you see this message when you add code to your git repository from the command prompt

```
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint: 
hint:   git submodule add <url> nodule
hint: 
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint: 
hint:   git rm --cached nodule
hint: 
hint: See "git help submodule" for more information.
```

## What's happening? 

You have added a git repository inside another git repository

The outer git repository will ignore the inner git repository.

The inner git repository is known as a submodule. 

Let's say you have these files and directories 

```
project 
   |- file1.html
   |- file2.css
   |- resources
       |- info1.json
       |- info2.json
```

If you create a git repository in the project directory, and there's a git repository in the resources directory, all the files inside the resources directory will be ignored by the git repository in the project directory. 
 
 A git repo inside another git repo is called a submodule. In other words, a directory with a git repository in, is inside another directory, also with a git repository in.  The submodule doesn't have to be in the immediate subdirectory, it can be one or two or more levels above. 
 
 Sometimes this is what you want to do, but if what you want to do is to collect files from more than one directory together in to one repository, you only want one git repository for the entire project. So a submodule is not what you want to happen.
 
 When a git repo is created, it created a hidden directory called .git and that's how the git tool knows it's working with a git repository. All of the info about your past versions of code, the location of the GitHub remote etc.. are stored in files in this .git directory. 
 
 If you have a git repo in the `project` directory, and another one in the `resources` directory, your file system will actually look like this,

```
 project 
   |- .git
   |- file1.html
   |- file2.css
   |- resources
       |- .git
       |- info1.json
       |- info2.json
```

If you add and commit files from the `project` directory, you'll see an entry for the `resources` directory under files you've added and committed, but the `info1.json` and `info2.json` files will not be added. 

If you want the project directory to have one git repo with everything in, follow these steps.

In the `resources` directory (the inner directory with a git repo in):  __delete the .git folder.__  You will need to enable hidden files to see this in explorer / finder.

Use a command prompt or git bash (windows) or terminal (mac, linux) and navigate to the `project` directory. You need to be in the directory above the one with the submodule - so in this example, the `project` directory (the outer directory, the one that should contain all the files). Then run the command 

`git rm --cached resources`  

but replace __resources__ with your own directory name.  The `--cached` part is really important, if you miss it out it will irreversably delete your `resources` directory!

Now you should be able to use the `git add` command to add all of the files in the resources directory to the main project's repository, and commit those files.

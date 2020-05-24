# README for livingpatterns.group website

## Installation for local testing

### Hugo

In order to test the website locally you need to have Hugo installed. Detailed instructions for all operating systems can be found here: [Hugo Installation Guide](https://gohugo.io/getting-started/installing/). Note: The website recommends installing Hugo on Linux via brew. This is not always required, e.g., if you work on Debian you can simply install it via:

    sudo apt install hugo

So check your package manager first. Brew is likely the easiest installation on OSX though. 

### Academic-admin

The academic-admin tool is required if you want to update / recreate the bibliography automatically. Detailed installation instructions can be found [here](https://github.com/sourcethemes/academic-admin). The tool can be installed via pip. Make sure that your pip is for python >= 3.6 (usually available as `pip` or as `pip3`). You can check which pip version you have by running:

    pip --version

This will show at the end what python version it uses. The academic-admin tool can then be installed with: 

    pip install -U academic-admin

Note: If no pip is installed on your system, you are likely on Windows and don't have any python environment available. You should check out [Anaconda](https://www.anaconda.com/products/individual) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html) and go down the rabbit hole from there.

### Git

Check if `git` is installed on your computer by running `which git` from the terminal. If nothing comes back, you'll have to install git. If you're on linux and git is not installed (very unlikely), check your package manager for git. 

On OSX and Windows go to the [git-scm](https://git-scm.com/) and install it. If you have never worked with git you should also check out the free course on git on [software carpentry](https://swcarpentry.github.io/git-novice/), the [ProGit book](https://git-scm.com/book/en/v2), or your favorite site to look stuff up. Either way, some basic git knowledge is assumed here and the instructions are written to ssh authenticate your system with your GitHub account: more details can be found [here](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh).

Furthermore, you can use graphical git clients, e.g., [GitHub Desktop](https://desktop.github.com/) or [Sublime Merge](https://www.sublimemerge.com/). Especially GitHub desktop is very well suited for beginners.


## Workflow for editing, adding, testing, and deploying the website

Here the general workflow is described for simple edits. More specific tools are described separately further below.

### Clone the website from GitHub

To clone the website from git hub to your local machine type the following into the terminal:

    git clone git@github.com:livingpatterns/livingpatterns-website.git

Note that you must have ssh keys setup (see above). You can also clone the website with a GUI client and an actual login. Up to you.
    

### Test the website locally

To test the website and changes you must have Hugo installed. If all good, just open a terminal, navigate to the livingpatterns folder that you have cloned and type:

    hugo server

This will, if all is well, serve the website so you can call it up on your `localhost` over port 1313. Open a browser and type in the following address:

    127.0.0.1:1313

This should display the website. The server can be stopped by pressing `Ctrl + C`.

### Structure of the website

All the contents of the website are stored in the `content` folder. In here you have `authors` where all the group members are stored (see below), the `home` folder which contains all the pages for the website, and more folder for all sub sites, one each. 

Images, files, etc. should be stored in `static`. This folder contains a subfolder for images and for files. Keep some structure here!

Website features such as the logo, the favicon, etc. are stored in the `assets` folder in the `images` subfolder. Style sheet configurations are stored in `assets/scss/custom.scss`. Note, these are only modifications and not a full theme. 

The themes that are activated are in `data`. There are two `toml` files, one for the fonts configuration in `fonts` and one for the theme / color configuration in `themes`. 

Re-implemented / changed widgets are in the `layouts` folder.

The general theme is stored in `themes/academic`. Do not change any files in here but rather re-implements assets, style sheets, layouts in the folders described above. 

The configuration of the website and the behavior and metadata are stored in `config/_default`. Mostly have a look at the `config.toml` and `params.toml` files. The `menus.toml` file contains the menu. These three files are excellently commented and should make sense automatically once you start looking through them.


### Editing files

Changing a file will automatically trigger a re-serving of the website, so ideally you don't have to do anything and can just let the hugo server running. If you get errors, you can always quit the server and try to restart it. If an error occurs trace it down until it works again.

You must familiarize yourself with how to create websites in Hugo using the academic theme. Check out the academic [documentation](https://sourcethemes.com/academic/docs/).


### Deploy the website

You must have been added to the livingspattern GitHub repository as a collaborator for this to work. Otherwise, you cannot directly push any changes. Talk to the PI if you have questions.

To deploy the site first make sure that it runs locally when you deploy it with Hugo on your machine. Then you need to stage the files that have been modified or newly added. Only stage the files that are required for the adding to the website by running:

    git add filename

Wildcards are allowed.

Check the status of the website by typing:

    git status

You should see that the files that you have added are green. Make sure that no unnecessary files get staged! 

When you are ready to deploy the website push it to GitHub by typing:

    git push

From here everything should go automatically. When a new version is pushed to GitHub, Netlify will automatically recreate the website and publish it online. Give it a few minutes and refresh your browser to check the website online. If the changes don't display try clearing your cache and cookies. If things still don't display check Netlify (you need the login for the livingpatterns GitHub account) or ask for help. Also check the repository on GitHub to see if everything has been pushed properly. You can find the repository [here](https://github.com/livingpatterns/livingpatterns-website). 


## Create Publication list

### Selecting featured sites

**Note**: The described approach will eradicate the featured publications list if the whole bibliography is overwritten. You will have to activate the featured publications again! For example, if you want to feature the Ramirez et al. (2017) paper, you have to open the file:

    content/publication/ramirez-2017/index.md

After the abstract it says

    featured: false

Set this to true and check offline if it is all good. Repeat this step for all manuscripts that you want to feature.

### Recreate bibliography from BibTeX

The bibliography BibTeX file is named `bibliography-livingpatterns.bib` and lives in the main folder, alongside with this readme file. You should add new publications to that file using a BibTeX manager, e.g., [jabref](https://www.jabref.org/). 

Ensure that all required information is filled out. Copy the abstract into the required field and save the BibTeX file. Make sure a `doi` number is present as well for each entry. Finally, ensure that no other, unnecessary links are filled in. These links will confuse the next steps. 
You can always repeat this step.

To automatically create the bibliography you must have the [academic-admin](https://github.com/sourcethemes/academic-admin) python tools installed. Open a terminal and navigate to the folder into which you cloned the `livingpatterns` repository from GitHub. The updated BibTeX file that you have just created should lay in this folder and should be named `livingpatterns-website.bib`. If all done, you can add the new content by running:

    academic import --bibtex guille-publications_bold.bib

If you want to recreate the whole biliography (careful with this!) run:

    academic import --bibtex guille-publications_bold.bib --overwrite

You should see a new folder in `content/publication/`. Make sure you update the featured publications list according to the instructions above. 

## Create a new group member

More detailed instructions can be found [here](). In brief, navigate to the livingpatterns folder and type:

    hugo new --kind authors authors/firstname-lastname

This will add a folder in `contents/authors/`. Navigate to that folder and open the `_index.md` file and adjust it to contain the information on the new group member. The full biography can be added underneath the last `---`. Use standard markdown formatting. 

## Links

Some useful links for managing all of these things:
 * [Hugo website](https://gohugo.io/)
 * [Hugo Academic theme](https://sourcethemes.com/academic/)
 * [Hugo Academic demo page](https://themes.gohugo.io/theme/academic/)
 * [Markdown cheat sheet](https://www.markdownguide.org/cheat-sheet)
 * [ProGit book](https://git-scm.com/book/en/v2)

You might also learn some stuff by looking at the [showcase](https://sourcethemes.com/academic/#expo). Some of those pages have their own github site where you can also look at the underlying code. You might want to check out [this website](https://alison.rbind.io/) and the associated [GitHub repository](https://github.com/rbind/apreshill) if you want to look at theme design, etc. This website is built differently from the livingpatterns one, however, there's always parallels. Look at the showcase on what is possible and then think really hard how it can be done.

## Some final notice

It might seem a bit overwhelming to manage this website at first, especially if you haven't worked with any of these tools before. However, you will learn so much more than just website design. Understanding and using git will help you throughout your whole academic career, because it is by far the default tool in order to keep your code version controlled. You might even want to read up more. 

You don't need to have any programming skills, but it sure doesn't hurt. Python is used in the academic-admin tool, you might want to look in more details into python for your work as well. Check out the courses on [software carpentry](https://software-carpentry.org/lessons/), they also have [one on python](https://swcarpentry.github.io/python-novice-inflammation/) as an introductory course. Also ask around, the group as a whole can also be a great resource for getting you started of.

And if it's too much, if you get stuck, always feel free to ask. Mistakes are normal, things go wrong, don't worry! This should be a fun task and hopefully teach you stuff, so don't despair, come by for a coffee, and we chat about problems, what went wrong, etc.

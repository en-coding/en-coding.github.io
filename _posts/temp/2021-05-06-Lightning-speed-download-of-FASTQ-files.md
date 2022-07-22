---
layout: post
title: Lightning speed download of FASTQ files
subtitle: Get large amount of data as fast as possible
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [fastq, NCBI, SRA]
---

This post is based on https://www.biostars.org/p/325010/


# Step 1: Install Aspera client

Actually you need to make a IBM account if you don't have it yet. You just need to register your email and validate with a an activate code. Then you're good to continue.

While is very confusing to choose which flavor you should pick up from the several versions of IBM products available [at the official website](https://www.ibm.com/products/aspera/downloads?lnk=STW_ZZ_STESCH&psrc=NONE&pexp=DEF&lnk2=learn_Aspera), [here](https://www.ibm.com/support/fixcentral/swg/selectFixes?parent=ibm~Other%20software&product=ibm/Other%20software/IBM%20Aspera%20CLI&release=All&platform=All&function=all) you have the shortcut for the use in the terminal, or [this one](https://www.ibm.com/support/fixcentral/swg/selectFixes?parent=ibm~Other%20software&product=ibm/Other%20software/IBM%20Aspera%20Desktop%20Client&release=All&platform=All&function=all) if you plan to use in a desktop.

If you chose the desktop, it is pretty much straightfoward, just install it following the guidelines and open the app to have it working.

But, if you are going to download directly to a server, you will need to download and install it properly to have things done. It is a bit more work but it can be done following the steps below.

Find and click on the right operating system. A window will open and you will see three links with download icons in the front. Right-click on the one that ends with *.sh (around 15MB). Click on 'copy link adress' or something similar (I'm using Mac here).

At your terminal: ssh to your server, and go to the directory you want to download the file. Anyplace is ok because when you execute it it will will install automatically at `/home/youruser/.aspera/cli`.

Then, download it using wget. Type 'wget' and paste the link you copied previously.

Example:
`wget https://ak-delivery04-mul.dhe.ibm.com/sar/CMA/OSA/08q6g/0/ibm-aspera-cli-3.9.6.1467.159c5b1-linux-64-release.sh`

After the download, give permission and execute the file.
`chmod +x ibm-aspera-cli-x.x.x.xxx.xxxxxxx-linux-xx-release.sh`
`ibm-aspera-cli-x.x.x.xxx.xxxxxxx-linux-xx-release.sh`

The installation is very fast and it will show how to include the PATH to your bash_profile file. Follow the instructions and copy+paste+run the command.
`export PATH=/home/youruser/.aspera/cli/bin:$PATH`

Done! You can check whether you did everything correctly by typing 'asc' and pressing tab to autocomplete. If it autocomplete to 'ascp' or show the option you did everything correct until this point. If not you may need to check where is your bashrc file and properly export. Please check [the first answer in this post](https://unix.stackexchange.com/questions/26047/how-to-correctly-add-a-path-to-path) for more instructions.


# Step 2: Download the dataset



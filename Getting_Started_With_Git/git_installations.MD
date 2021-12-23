# Installing Git
Before you start using Git, you have to make it available on your computer. Even if it’s already installed, it’s probably a good idea to update to the latest version. You can either install it as a package or via another installer, or download the source code and compile it yourself


#### Installing on Linux
1. If you want to install the basic Git tools on Linux via a binary installer, you can generally do so through the package management tool that comes with your distribution. If you’re on Fedora (or any closely-related RPM-based distribution, such as RHEL or CentOS), you can use :
   ```sh
   yum install git -y  
   ```
If you’re on a Debian-based distribution, such as Ubuntu, try apt
   ```sh
   sudo apt install git-all 
   ```
For more options, there are instructions for installing on several different Unix distributions on the Git website, at https://git-scm.com/download/linux.


#### Installing on macOS
1. There are several ways to install Git on a Mac. The easiest is probably to install the Xcode Command Line Tools. On Mavericks (10.9) or above you can do this simply by trying to run git from the Terminal the very first time.
   ```sh
   git --version 
   ```
If you’re on a Debian-based distribution, such as Ubuntu, try apt
   ```sh
   sudo apt install git-all 
   ```
If you don’t have it installed already, it will prompt you to install it.
If you want a more up to date version, you can also install it via a binary installer. A macOS Git installer is maintained and available for download at the Git website, at https://git-scm.com/download/mac.


#### Installing on Windows 
There are also a few ways to install Git on Windows. The most official build is available for download on the Git website. Just go to https://git-scm.com/download/win and the download will start automatically. Note that this is a project called Git for Windows, which is separate from Git itself; for more information on it, go to https://gitforwindows.org.

To get an automated installation you can use the Git Chocolatey package. Note that the Chocolatey package is community maintained.

# About Git usernames
You can change the name that is associated with your Git commits using the git config command. The new name you set will be visible in any future commits you push to GitHub from the command line. If you'd like to keep your real name private, you can use any text as your Git username.

Changing the name associated with your Git commits using git config will only affect future commits and will not change the name used for past commits.

#### Setting your Git username for every repository on your computer 
1) Open Git Bash.
2) Set a Git username:
   ```sh
   git config --global user.name "Mona Lisa"
   ```
3) Confirm that you have set the Git username correctly:
   ```sh
   git config --global user.name
   > Mona Lisa
   ```

 #### Setting your commit email address
 You can set the email address that is used to author commits on GitHub.com and on your computer using the this link: 
 https://docs.github.com/en/articles/setting-your-commit-email-address
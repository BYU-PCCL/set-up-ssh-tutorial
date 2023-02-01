# PCCL SSH Setup Tutorial for GitHub access on Sivri
A tutorial for new lab members on how to set up SSH access to github on sivri without messing up any other lab members SSH connections.

## Step 1: Create a new SSH key
1. Open a terminal and SSH into sivri `ssh remote@sivri`
1. Type `cd ~/.ssh` to navigate to the SSH directory
1. Generate an SSH key (use your email): `ssh-keygen -t rsa -C "<your.email@example.com>"`
1. The key generator will ask you for a filename.
    - Make your filename `id_rsa_<github_username>` (e.g. `id_rsa_johnsmith`)
1. Enter a passphrase (you will need to enter this passphrase every time you use the key). You can leave it blank if you want, but then anyone on sivri has access to your GitHub account.

## Step 2: Add your SSH key to your GitHub account
1. In your terminal, type `cat id_rsa_<github_username>.pub` to print the public key to the terminal.
1. Copy the public key to your clipboard (highlight the text and press `ctrl + c`)
1. Go to your GitHub account settings
    - Click on the `SSH and GPG keys` tab
    - Click on `New SSH key`
    - Give your key a title (e.g. `PCCL Sivri`)
    - Paste your public key into the `Key` box
    - Click `Add SSH key`

## Step 3: Add your SSH key to your sivri config file
1. Go back to your terminal and type `nano config` to open the config file in an editor
1. Append the following lines to the bottom of the config file:
    ```
    # <Your Name>'s github ssh key
    Host github.com-<github_username>
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa_<github_username>
    ```
1. Save the file and exit the editor
1. Test your connection by typing `ssh -T github.com-<github_username>`. You should see a message like `Hi <github_username>! You've successfully authenticated, but GitHub does not provide shell access.`

## Step 4: Update your remote URLs
1. Now, for every repository you own, you should update the remote url:
    - `$ git remote set-url origin git@github.com-<github_username>:<path_to_your_repo>.git`
    - For example, if I wanted to update the remote url for the `set-up-ssh-tutorial` repository, I would type:
        - `$ git remote set-url origin git@github.com-alexgshaw:BYU-PCCL/set-up-ssh-tutorial.git`
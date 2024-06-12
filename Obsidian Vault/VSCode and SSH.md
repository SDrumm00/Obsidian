How to connect to [[GitHub]] using a secure method like SSH.

https://awsm.page/git/use-github-with-ssh-complete-guide-including-vscode-setup/

# Use GitHub with SSH - Complete guide including VSCode setup

If you want to use Git without using password, then SSH key is the solution. Using SSH key is highly recommended and followed by professional developers often. In this post I will walk you through the setup of SSH keys for your GitHub account. And the process will be similar for any other services like Gitlab/Bitbucket also.

**Note:** All of the following commands are to be run in Git Bash. And now that you started using SSH keys, **never** use your password unless you login in browser. I recommend to delete all your personal access tokens from your account, visit [Tokens](https://github.com/settings/tokens) and delete all.

## Before you begin

Ensure you have git installed. On [[Debian]] that command looks like this.
```bash
sudo apt install git
```

## Configure basic user info

First you need to tell git your user name and email address. This can be user name and email address with which you want to make commits.

```bash
git config --global user.name "YOUR_USERNAME"
git config --global user.email "YOUR_EMAIL_ADDRESS"
```

### Testing the config

You can verify the configured username and email using the following commands

```bash
git config --global user.name
git config --global user.email
```

I recommended to config the line endings and case sensitivity for file names using `git config --global core.autocrlf true` and `git config --global core.ignorecase true`.

## Setting up SSH keys

### First check if there are any keys

To check if there are any existing SSH keys on your PC:

```bash
ls -al ~/.ssh
```

If you don't have an existing public and private key pair, or don't wish to use any that are already available to connect to GitHub, then see the below steps to generate a new SSH key.

If you receive an error that `~/.ssh doesn't exist`, don't worry! We'll create that folder by generating a new SSH key

## Generate a new SSH key

To create a new SSH key, run the following command substituting in your GitHub email address.

```bash
ssh-keygen -t rsa -b 4096 -C "YOUR_GITHUB_EMAIL_ADDRESS"
```

Leave the first question about where to store the SSH Key in the default folder and hit Enter.

You can use a passphrase, for the SSH key being generated. Just enter the passphrase when the CLI prompts.  I typically leave mine blank. This is **NOT** same as the password that you use to login GitHub.

## Connecting the generated SSH key to GitHub

To connect the generated SSH key to your GitHub profile, you have to add it in the account settings of your account. To do so:

- [x] Navigate to [**SSH and GPG keys**](https://github.com/settings/keys) section.
- [x] Click on New SSH key
- [x] Give some title, (recommended: that you can identify your PC with).
- [x] Paste the generated SSH key
- [x] Click on Add SSH key

To copy the generated SSH key to clipboard, use the **clip** command

```bash
cat ~/.ssh/id_rsa.pub
```

## Testing the whole setup

Run the following command, to test your entire SSH key setup.

```plaintext
ssh -T git@github.com
```

**Note:** To use SSH keys effectively, you have use the SSH protocol while cloning a repo, or adding a remote to existing repo

```bash
# Avoid this, or it will ask for your password
git clone https://github.com/awsm-page/sample.git

# Do this
git clone git@github.com:awsm-page/sample.git
```

Similarly, you have to consider existing repos which are previously authenticated with _personal access tokens_ also.

Check the remotes of any repository you work with, whether they are using HTTP or SSH.

```bash
# List all remotes
git remote -v

# Change to SSH remote, if needed
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

Complete guide for switching URLs can be found at [GitHub docs](https://help.github.com/en/github/using-git/changing-a-remotes-url#switching-remote-urls-from-https-to-ssh)

## Adding your SSH key to the ssh-agent(optional)

You can add your SSH key to the ssh-agent, if you don't want reenter your passphrase every time you use your SSH key. To do so:

Start the ssh-agent in the background, using the command

```bash
eval "$(ssh-agent -s)"
```

Run the following command to delete all identities from the agent

```bash
ssh-add -D
```

Then add the generated key to the agent

```bash
ssh-add ~/.ssh/id_rsa
```

To list the added keys:

```bash
ssh-add -l
```

You can also read [GitHub's guide to SSH key and SSH agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

## Setup VSCode

VS Code works most easily with SSH keys without a passphrase. To use SSH Git authentication with VS Code, you have to launch VS Code from a Git Bash prompt to inherit its SSH environment using one of the below commands.

```bash
code .
# or
code path/to/specific/folder
```

If you are on Mac OS or Linux, you can run the command from any terminal.
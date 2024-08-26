# Setup Bitbucket Access With Multiple Users

This post denotes how to setup and authenticate multiple users acrosss multiple remote repository management platforms like GitHub, Bitbucket, GitLab, etc. While the exact procedure is tailored towards bitbucket, I believe you can apply the principles to any combination of account providers.

## Why

Typically, when working on a single device, to enable cloning a bitbucket repository via ssh is simple: You create a new ssh key on that device, and register the public part of the key as a new device under your Bitbucket work account settings. Then all you need is to clone the repo as normal and it should authenticate over ssh.

BUT

Sometimes you may run into a situation where multiple users need to work on a single development server but have their own unique ssh keys for authenticating to a remote Bitbucket Repository.

In my case, a team member and myself wanted to clone and work on our assigned bitbucket repositories on a single development server. THat meant generating new ssh key files for each unique user, registering those keys on our work Bitbucket acounts respectively, and setting up aliases using an ssh config file as well as modifying the remote repository url in our cloned repositories.

## How

Step by Step, the procedure is as follows:

1. Create an SSH key corresponding to each unique account.

   ```console
   ssh-keygen -t ed25519 -b 4096 -C "{username@domain.com}" -f {ssh-key-name}
   ```

   In my case this would look like:

   ```console
   ssh-keygen -t ed25519 -b 4096 -C "{arslan.kazmi@<company>.com}" -f ak-<company>-bitbucket
   ```

   make sure to check that the ssh keys are registered to your device or OS ssh agent

   ```console
   ssh-add ~/{ssh-key-name}
   ```

2. Create a corresponding SHH config file entry:

   ```console
   #bitbucket_username1 account
   Host bitbucket.org-bitbucket_username1
   HostName bitbucket.org
   User git
   IdentityFile ~/.ssh/{ssh-key-bitbucket_username1}
   IdentitiesOnly yes
   ```

   I typically used a modified format for naming my "Host" entry as follows, to make it simpler:

   ```console
   #bitbucket-arslan
   Host bitbucket-arslan
   HostName bitbucket.org
   User git
   IdentityFile ~/.ssh/ak-<company>-bitbucket
   IdentitiesOnly yes
   ```

3. Add the public part of the SSH keys generated in Step 1 to your Bitbucket account under ssh keys (the filename that  ends with .pub). Follow the instructions on this link: https://support.atlassian.com/bitbucket-cloud/docs/set-up-personal-ssh-keys-on-linux/#Provide-Bitbucket-Cloud-with-your-public-key

Aaaaand, you're done! 

Now you can freshly clone a remote repository using the host you provided in your ssh config file, or update the remote reference in an already cloned repository to match the new host entry.

For fresh cloning:

```console
git clone git@bitbucket.org-{bitbucket_username}:{workspace}/{repo}.git
```

In my case this would look like:

```console
git clone git@bitbucket-arslan:{workspace}/{repo}.git
```

To update an already cloned local copy of a repository:

```console
git remote set-url origin git@bitbucket.org-{bitbucket_username}:{workspace}/{repo}.git
```

In my case,

```console
git remote set-url origin git@bitbucket-arslan:{workspace}/{repo}.git
```

>You can also manually edit the entry in the config file in your local repository's .git folder.

## Thanks

I took a large inspiration from Kat Connely's blog post on using multiple ssh keys per unique remote repository service. The exact mechanisms specific to my bnitbucket use case were then applied according to Atlassian's excellent documentation. Both of these are linked below.


References:

1. https://connkat.medium.com/setting-up-multiple-ssh-keys-on-one-computer-75f068d972d9
2. https://support.atlassian.com/bitbucket-cloud/docs/managing-multiple-bitbucket-user-ssh-keys-on-one-device/

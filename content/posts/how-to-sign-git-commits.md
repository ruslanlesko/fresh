---
title: "How to sign git commits"
date: 2022-10-05T13:30:00+01:00
description: "Setting up GPG keys to sign your git commits"
tags: [Tools, GitHub]
---
There is a feature in git which allows signing commits by GPG key. However, not all developers heard about it. In this post, we will see how to enable it.

## Why do you need it?

The only purpose of signing your commit is for others to be sure that this commit was really made by you and not by someone pretending to be you (anyone can set a completely random name and email by `git config user.name 'Linus Torvalds'` and `git config user.email 'torvalds@linux-foundation.org'`). 

Also, signed commits are marked as verified by git hosting providers such as GitHub or GitLab.

![List of commits on GitHub](/list_of_commits_on_github.png "List of commits on GitHub")

## Create GPG key

The first thing you need to do is to create a GPG key if you don't already have it. Check the version of GPG installed by executing

```bash
gpg --version
gpg (GnuPG) 2.3.7
```

If you have GPG version higher than 2.1.17, then generate the key by executing

```bash
gpg --full-generate-key
```

Otherwise, use the following command

```bash
gpg --default-new-key-algo rsa4096 --gen-key
```

It will prompt you to enter your name, email, key expiration date, and passphrase. I strongly recommend having a passphrase to protect your identity even if the key will be leaked.

## Configure git to sign each commit

After you generated a key, it's time to tell git about it. You need a key ID for that. Execute the following to find it.

```bash
gpg --list-secret-keys --keyid-format=long
/Users/ruslan/.gnupg/secring.gpg
------------------------------------
sec   4096R/399E18176B5EF6D2 2022-01-10 [expires: 2026-01-10]
uid                          Ruslan Lesko <ruslanlesko@gmail.com.com>
ssb   4096R/AF97D7A7275DE23 2022-01-10
```

In the example above, the key ID is `399E18176B5EF6D2`. Copy that and paste it into our next command:

```bash
git config --global user.signingkey 399E18176B5EF6D2
```

As you can see, I am using a `--global` parameter to apply this config to all my repositories. Alternatively, you can set different keys for different repositories (in case you have different email accounts, for example).

Now we can set a parameter to sign all commits by default.

```bash
git config --global commit.gpgsign true
```

As with the previous command, you can omit `--global` param to apply this config only for a specific repository.

If you don't want to sign each commit by default, then for signing commits you should use `-S` parameter. For example.

```bash
git commit -S -m 'This commit is signed'
```

For some systems, you may encounter an issue with the passphrase prompt not getting opened. For example, on mac. In this case, go to your `.bashrc` (or `.zshrc` if you are on a mac) to add this line and restart your terminal:

```bash
export GPG_TTY=$(tty)
```

## Add GPG public key to git hosting provider

If you want GitHub, GitLab, or any other git provider to label signed commits as verified, you need to associate the public GPG key with your account. For that, we will need a public portion of the previously generated key. To export it, you will need a key ID, which we saw previously.

```bash
gpg --armor --export 399E18176B5EF6D2
```

Then, copy the output and paste it into the GPG section in the setting menu of your git provider.

![Adding GPG key on GitHub](/adding_gpg_key_on_github.png "Adding GPG key on GitHub")

Congrats, you have completed your git GPG setup!
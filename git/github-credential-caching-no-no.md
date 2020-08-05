---
description: >-
  Many people use GitHub credential manager to cache their credentials to avoid
  having to re-enter their username and passwords each time.
---

# GitHub credential caching no no

[https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

## Danger Danger Danger

On 14 April 2020 a vulnerability was found in the github credential manager that may cause Git to present stored credentials to the wrong server. That wrong server could be waiting and saving those credentials.

> Specially-crafted URLs that contain an encoded newline can inject unintended values into the credential helper protocol stream, causing the credential helper to retrieve the password for one server \(e.g., `good.example.com`\) for an HTTP request being made to another server \(e.g., `evil.example.com`\), resulting in credentials for the former being sent to the latter. There are no restrictions on the relationship between the two, meaning that an attacker can craft a URL that will present stored credentials for any host to a host of their choosing.

### Basically stop using github credential manager to store your credentials, instead opt for using a SSH key to access your Repos.



## Step 1 - Generate an SSH key

1. Generate a new SSH key reserved for Github.
2. Yes like WAX dont use the same keys for too many actions.

## Step 2 - Associate your SSH key with the remote repository

1. In your Github Repo - go to [**settings**](https://github.com/settings/ssh) and click 'add SSH key'. Copy the contents of your `~/.ssh/id_rsa.pub` into the field labeled 'Key'

## Step 3 - Change your remote URL to support SSH

1. Check your remove URL like so.

```text
git remote show origin
```

2. Change yor URL to SSH.

```text
git remote set-url origin git+ssh://git@github.com/username/reponame.git
```


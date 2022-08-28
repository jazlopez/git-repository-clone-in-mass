# git-repository-clone-in-mass
Upon an organization name clone all available repositories to your computer


Background:

In github.com there are a lots of organization that offers code templates, exercies, and material for courses and tranining online and it comes handry the posibility of clone the entire set of repositories available (private repositories may be exluded per the owner of the organization account but it works for all public repositories).

What you need to setup before get started in github CLI API also known as GH. You can download from the official page https://github.com/cli/cli#installation
Once your GH client is installed in your system you need to proceed to authenticate yourself to github.com via creating a user personal token so the client can impersonate you and access the repositories for you. Such process of getting a token to impersonate github api calls is described here https://cli.github.com/manual/gh_auth_login

Now it is time to collect some repositories from an organization.
I will use as example this organization https://github.com/quick-tutorials-with-aws-cloud
And my goal is to clone all public repositories into my local

First step is capture the clone url for each of the repositories within the organization

```
gh api --paginate \
  -H "Accept: application/vnd.github+json" \
  /orgs/quick-tutorials-with-aws-cloud/repos | jq -r '.[].ssh_url' | nl
```

The above example will output the git clone ssh urls to your screen. Lets modify the script to save the list into a  text file file

```
gh api --paginate \"Accept: application/vnd.github+json" \
  -H "Accept: application/vnd.github+json" \
  /orgs/quick-tutorials-with-aws-cloud/repos | jq -r '.[].ssh_url' | nl > quick.tutorials.with.aws.cloud.txt
```

Since the list is enumerated by the `nl` command and we are only interested in the ssh git clone URL lets strip off the enumeration and start cloning the repositories. 

This is the final step of this practice

```
for repository in `cat quick.tutorials.with.aws.cloud.txt | awk '{print $2}'`; do git clone $repository; done;
```

Now you are able to clone the available repositories from any organization whose repos are public to the outside world.

#### AUTHOR

Jaziel Lopez <jazlopez@github.com>

#### VERSION

1.0.0 Initial (and prob.the final)
    Q3 2022





# Refinery CMS Quickstart for OpenShift
This is an application template for [OpenShift][openshift],
using [Refinery CMS][refinerycms].

Refinery is a content management system written on Rails.
As such, be sure to review [OpenShift's Rails Quickstart][rails-quickstart]
for further information on how OpenShift's architechture affects your
application.

## How-To
This repository is based on the Ruby 1.9 application template that OpenShift
generates when you run

    rhc app create -a <app> -t ruby-1.9

To get you up on Refinery, follow the steps below.
For the purpose of clarity, we use `mycms` as your Refinery-based application
name.

1. Create Ruby 1.9 application on OpenShift

        rhc app create -a mycms -t ruby-1.9
    
    This creates directory `mycms` under the current directory.

1. Add PostgreSQL as the database

        rhc app cartridge add -a mycms -c postgresql-8.4

1. Change directory to `mycms`

        cd mycms
        rm -rf *

1. Add the `quickstart` remote. (Alternately, you can clone this repo and use the clone's git URL.)

        git remote add quickstart -m master git://github.com/BanzaiMan/refinerycms-quickstart.git

1. Apply `quickstart`'s changes to the local repository

        git pull -s recursive -X theirs quickstart master
    
1. Push the code to your OpenShift application

        git push
    
    This can take a few minutes while assets are recompiled,
    databases are initialized, and Passenger warms up.

1. Visit your application and set up Refinery CMS account.

    http://mycms-yournamespace.rhcloud.com
    
    You should see a screen like this:
    
    ![Refinery initial setup screen](https://img.skitch.com/20121005-pp6wj7kbdiri4ukhsf9b7kqymj.png)

## Caveat Emptor
Notice that this repository has a nontrivial `.openshift/action_hooks/deploy`.
The hook looks for a semaphore file and sets up the database if the file does
not exist.
While some care is taken for the action to be non-destructive on subsequent
code pushes, it is possible that the semaphore file gets lost (by error or
malice).
If this worries you, it is advisable that you comment out the hook's code once
the database is set up.

[openshift]: http://openshift.com
[refinerycms]: http://refinerycms.com 
[rails-quickstart]: https://github.com/openshift/rails-example
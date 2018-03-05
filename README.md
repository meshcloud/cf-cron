# Running Cron Jobs on Cloud Foundry

This repository demonstrates how to run scheduled tasks on Cloud Foundry with a very small footprint (8 to 16 MB RAM) using a traditional crontab. This means you can run it for a few cents of monthly cost 

Traditional cron daemons need to run as root and have opinionated defaults for logging and error notifications. This makes them unsuitable for running in a containerized environment like Cloud Foundry. Instead of a system cron daemon we're thus using [supercronic](https://github.com/aptible/supercronic) to run our cron tab. 

## How it works

This application is built using the binary buildpack and executes `supercronic` on the `crontab` file. The `crontab`file specifies all your cron jobs. To add additional jobs, simply add a new line  which specifies a schedule and command to the `crontab`. 

> Note: By default, `supercronic` will log [all output to stderr](https://github.com/aptible/supercronic/issues/16) so we redirect that to stdout in our cf manifest. 

You can also include additional scripts and binaries to execute more complex actions. This example allows you to install apt and debian packages to use in your cronjobs. You can specify these packages in `apt.yml` and they will be installed during staging by [apt-buildpack](https://github.com/cloudfoundry/apt-buildpack)
courtesy of the magic [multi-buildpack](https://github.com/cloudfoundry/multi-buildpack).

## Running Sceduled Tasks on Cloud Foundry

While the cron container here is designed to be small and lightweight, you may want to use it to trigger more resource intensive tasks and processes. When a simple `curl` to an http endpoint is not enough to kick off such a task on your existing app, [Cloud Foundry Tasks](https://docs.cloudfoundry.org/devguide/using-tasks.html) are a great solution to run these processes. 

This sample repository thus includes instructions to install the `cf` cli tool which you can use to trigger such a task using a [Meshcloud Service User](https://meshcloud.gitbooks.io/meshcloud/content/meshcloud/service-user.html).
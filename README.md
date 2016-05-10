# hubot-packager
A vagrant project to package a customised [hubot](https://hubot.github.com/)
into an RPM suitable for deployment on RHEL/CentOS 7.

## Details
This vagrant project will package hubot into an RPM, including selected external
scripts and packages.  The resulting RPM package will:

  - Deploy hubot into the specified directory on the target system.
  - Create a configuration file `/etc/sysconfig` containing hubot's environment
    variables.
  - Create a dedicated system user and group for running the bot.
  - Create a systemd unit file so that hubot can be controlled with the standard
    service management commands, e.g.:

```bash
systemctl start hubot-bigcorp
systemctl stop hubot-bigcorp
systemctl restart hubot-bigcorp
systemctl status hubot-bigcorp
systemctl enable hubot-bigcorp
systemctl disable hubot-bigcorp
```

## Build requirements

1. [Vagrant](https://www.vagrantup.com/).
2. A hypervisor. I used [VirtualBox](https://www.virtualbox.org/).

## Configuration
Adapt the provided `.conf.example` files to create your custom bot:

 - `config.conf` should contain package configuration options.
 
 - `env.conf` should contain all environment variables used by your bot and its
   scripts.  Be sure to include the `EXPRESS_PORT` and `PATH` variables from the
   `.env.conf.example` file.
   
   *N.B.* It is not advised to include secrets such as
   API keys and passwords in your RPM file. Instead add these to the file on the
   target system by hand or using your favourite systems configuration utility.
   
 - `external-scripts.conf` should contain a list of npm hubot scripts to include
   with your bot.
 
 - `packages.conf` should contain a list of other npm packages to include with
   your bot.

Any coffee scripts placed in `scripts` will also be packaged.
 
## Target system requirements
The built RPM package is suitable for deployment on EL 7 (RHEL, CentOS, etc).
The package is built with two specified dependencies:

1. redis (no version specified.  The version in the
[EPEL](https://fedoraproject.org/wiki/EPEL) repo works fine.
2. nodejs >= 5 or above. I used the
[nodesource repo](https://rpm.nodesource.com/pub_5.x/el/7/x86_64/)

## Build

Initial build:

```bash
vagrant up
```
Subsequest rebuilds:

```bash
vagrant provision
```

Be sure to increment your `BOT_RELEASE` number in `config.conf` before
rebuilding.

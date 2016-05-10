# hubot-packager
A vagrant project to package a customised [hubot](https://hubot.github.com/)
into an RPM suitable for deployment on RHEL/CentOS 7.

## Details
This vagrant project will package hubot into an RPM, including selected external
scripts and packages.  The resulting RPM package will:

  - Deploy hubot into the specified directory on the target system, e.g. 
    `/opt/hubot-[your_suffix]`
  - Create a configuration file `/etc/sysconfig/hubot-[your_suffix]` containing
    hubot's environment variables.
  - Create a dedicated system user and group for running the bot.
  - Create a systemd unit file so that hubot can be controlled with the standard
    service management commands, e.g.:

```bash
systemctl start hubot-[your_suffix]
systemctl stop hubot-[your_suffix]
systemctl restart hubot-[your_suffix]
systemctl status hubot-[your_suffix]
systemctl enable hubot-[your_suffix]
systemctl disable hubot-[your_suffix]
```

## Build requirements

1. [Vagrant](https://www.vagrantup.com/).
2. A hypervisor. I used [VirtualBox](https://www.virtualbox.org/).

## Download

```bash
git clone https://github.com/liger1978/hubot-packager.git
```

Or download
[release archive](https://github.com/liger1978/hubot-packager/releases) and
unzip.

## Configure
Adapt the provided `.conf.example` files to create your custom bot:

 - `config.conf` should contain package configuration options.
 
 - `env.conf` should contain all environment variables used by your bot and its
   scripts.  Be sure to include the `EXPRESS_PORT` and `PATH` variables from the
   `.env.conf.example` file.
   
   *N.B.* It is not advised to include secrets such as
   API keys and passwords in your RPM file. Instead add these to the file on the
   target system by hand or using your favourite systems configuration utility.
   
 - `external-scripts.conf` should contain a list of npm
   [hubot scripts](https://www.npmjs.com/search?q=hubot) to include with your
   bot.
 
 - `packages.conf` should contain a list of other npm packages to include with
   your bot.

Any [.coffee](http://coffeescript.org/) scripts placed in the `scripts`
directory will also be packaged.

## Build

Initial build:

```bash
cd hubot-packager
vagrant up
```
Subsequent rebuilds:

```bash
vagrant provision
```

Be sure to increment your `BOT_RELEASE` number in `config.conf` before
rebuilding.

## Target system requirements
The built RPM package is suitable for deployment on EL 7 (RHEL, CentOS, etc).
The package is built with two specified dependencies:

1. `redis` (no version specified).  The version in the
[EPEL](https://fedoraproject.org/wiki/EPEL) repo works fine.
2. `nodejs >= 5`. I used the
[nodesource repo](https://rpm.nodesource.com/pub_5.x/el/7/x86_64/)
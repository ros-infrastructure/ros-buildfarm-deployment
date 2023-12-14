# ROS buildfarm deployment

Scripts in this repository will automate the bootstrap and setup of ROS build farm hosts.

## Supported platforms

* Ubuntu 20.04

## Prerequisites

These scripts must be run as root on hosts that meet the [requirements for a ROS build farm deployment][requirements].
Before running the scripts you must have a [configured chef repository][chef-repo-setup].

[requirements]: # TODO
[chef-repo-setup]: # TODO

## Usage

1. Clone this script repository to your target host.
2. Copy `configuration.bash.example` into `configuration.bash` and populate with the necessary values. 
3. Run `./bootstrap`
4. Run `./configure CHEF_REPO CHEF_BRANCH CHEF_ENVIRONMENT CHEF_SOLO_FILE`

To refresh a host which has been configured previously

1. Run `./configure`

## Connecting to authenticated repositories

Your chef repository will contain sensitive contents such as ssh private keys, gpg private keys, passwords, and tokens.
Making that information public is strongly discouraged.

SSH agent forwarding can be used to grant temporary access to the configuration repository for the duration of configuration.
1. Set up your local ssh-agent
2. Add a key to your local agent which has access to your chef repository
3. Connect to your build farm host with ssh agent forwarding enabled using the `-A` command line flag or `ForwardAgent yes` configuration option
4. If using `sudo` for an interactive root shell use `sudo -Es` to preserve the environment variables containing SSH agent socket information.

## Scripts
All scripts populate their environment variables through `configuration.bash`. Some of these configurations can be overwritten through args.

### [bootstrap](./bootstrap)

Boostrap the necessary dependencies to configure the agent:
- Chef
- Git
- Curl



#### Examples
Overrides configuration value with custom `CHEF_VERSION`
```
sudo ./bootstrap CHEF_VERSION
```
Skips manual validation of `OMNITRUCK` script execution.
```
sudo RUN_OMNITRUCK=1 ./bootstrap 
```

### [configure](./configure)
Executes the chef cookbooks and configures the agent.

#### Examples
This overrides all values with the corresponding args values passed on execution.

```
./configure CHEF_REPO CHEF_BRANCH CHEF_ENVIRONMENT CHEF_SOLO_FILE
```
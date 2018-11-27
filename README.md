# saltstack-learning-windows

## Description

This repo is the Hyper-V equivalent of [the repo](https://github.com/UtahDave/salt-vagrant-demo)
that is linked to in the [SaltStack Fundamentals](https://docs.saltstack.com/en/getstarted/fundamentals/index.html) tutorial.

The `Vagrantfile` contained in this project will stand up a Salt master and two
minions, automatically connecting the them together and preseeding the master.

## Motivation

The primary motivator was to allow a Windows SaltStack user (a chemist?) to dive
into SaltStack without having to deal with having two hypervisors (both VBox and
Hyper-V), especially if you're also using something like Docker for Windows.

Another motivator is just a general preference for native tooling when possible.
Obviously there is less of a community around Hyper-V given that it is exclusive
to the operating system that people love to hate on, but it's a very solid product
and generally feels like less of a hack or kludge than VirtualBox. Obviously there
are differing opinions on this.

## Instructions

1. Open up the `Vagrantfile` and take a look at the config section to make sure
   that it's sane for you (for example, you actually have >6GB RAM), etc. If
   you update `num_minions` to larger than `2`, you will need to generate
   additional keys.
1. Bring up the master via `vagrant up master`. Make sure to note the IP address.
1. Update the `Vagrantfile` with the master's IP address.
1. Bring up the minions via `vagrant up minion1 minion2 --parallel`.

# driverkit

[![Latest](https://img.shields.io/github/v/release/falcosecurity/driverkit?style=for-the-badge)](https://github.com/falcosecurity/driverkit/releases/latest)
![Architectures](https://img.shields.io/badge/ARCHS-x86__64%7Caarch64-blueviolet?style=for-the-badge)

A command line tool that can be used to build the [Falco](https://github.com/falcosecurity/falco) kernel module and eBPF probe.

## Glossary

When you meet `kernelversion` that refers to the version you get executing `uname -v`:

For example, below, the version is the `59` after the hash

```bash
uname -v
#59-Ubuntu SMP Wed Dec 4 10:02:00 UTC 2019
```

When you meet `kernelrelease`, that refers to the kernel release you get executing `uname -r`:

```
uname -r
4.15.0-1057-aws
```

## Help

By checking driverkit help, you can quickly discover info about:
* Supported architectures
* Supported targets
* Default options

```
driverkit help
```

## Architecture

The target architecture is taken from runtime environment, but it can be overridden through `architecture` config.  
Driverkit also supports cross building for arm64 using qemu from an x86_64 host.

Note: we could not automatically fetch correct architecture because some kernel names do not have the `-$arch`, namely Ubuntu ones.

## How to use

### Against a Kubernetes cluster

```bash
driverkit kubernetes --output-module /tmp/falco.ko --kernelversion=81 --kernelrelease=4.15.0-72-generic --driverversion=master --target=ubuntu-generic
```

### Against a Docker daemon

```bash
driverkit docker --output-module /tmp/falco.ko --kernelversion=81 --kernelrelease=4.15.0-72-generic --driverversion=master --target=ubuntu-generic
```

### Build using a configuration file

Create a file named `ubuntu-aws.yaml` containing the following content:

```yaml
kernelrelease: 4.15.0-1057-aws
kernelversion: 59
target: ubuntu-aws
output:
  module: /tmp/falco-ubuntu-aws.ko
  probe: /tmp/falco-ubuntu-aws.o
driverversion: master
```

Now run driverkit using the configuration file:

```bash
driverkit docker -c ubuntu-aws.yaml
```

### Configure the kernel module name

It is possible to customize the kernel module name that is produced by Driverkit with the `moduledevicename` and `moduledrivername` options.
In this context, the _device name_ is the prefix used for the devices in `/dev/`, while the _driver name_ is the kernel module name as reported by `modinfo` or `lsmod` once the module is loaded.

### Examples

For a comprehensive list of examples, heads to [example configs](Example_configs.md)!

## Survey

We are conducting a [survey](http://bit.ly/driverkit-survey-vote) to know what is the most interesting set of Operating Systems we must support first in driverkit.

You can find the results of the survey [here](http://bit.ly/driverkit-survey-results)
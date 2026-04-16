# GPU acceleration
GPU acceleration is only available for servers that fulfill [the prerequisites](#prerequisites). If the server does, system administrators need to follow the [required steps to activate the GPU acceleration](#activation-steps).

## Prerequisites
To enable GPU acceleration, the server running OMERO must have the following:

- an NVIDIA GPU
- podman version 4.1 or higher

For podman to recognize the GPU, you also need to follow the steps described in the section below.

>Ubuntu 22.04 does not natively serve the required podman version. 
> To install it anyway, run the following script **as root**:
{style="warning"}
```bash
#!/bin/sh

ubuntu_version='22.04'
key_url="https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/unstable/xUbuntu_${ubuntu_version}/Release.key"
sources_url="https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/unstable/xUbuntu_${ubuntu_version}"

echo "deb $sources_url/ /" | tee /etc/apt/sources.list.d/devel:kubic:libcontainers:unstable.list
curl -fsSL $key_url | gpg --dearmor | tee /etc/apt/trusted.gpg.d/devel_kubic_libcontainers_unstable.gpg > /dev/null
apt update
apt install podman
```

## Activation steps

### Step 1 - Let podman access GPU(s)
Follow the instructions from the [official podman documentation](https://podman-desktop.io/docs/podman/gpu).

### Step 2 - Configure J2O OMERO settings
To prevent J2O containers to access random GPU processing power, the plugin provides some OMERO configurations to control GPU access.

#### GPU devices
System administrators can choose which GPU devices will be available to J2O. Check the generated  CDI file (/etc/cdi/nvidia.yaml) to find the exact name of a device. Device names need to be added to the OMERO config **as a comma-seperated string** using the respective command, for example:

```bash
omero config set omero.web.jipipe.gpu_devices "nvidia.com/gpu=all"
```

> If no device is given, no GPU will be accessible!
{style="warning"}

#### GPU count
If we just give podman containers a list of GPU devices, GPU access would be random. To enable a scheduler that tracks whether a GPU is already in use for another J2O workflow, system administrators can specify the number of GPUs that are being used. For example, to enable the scheduler for one GPU we use the command:

```bash
omero config set omero.web.jipipe.gpu_count 1
```

To disable the scheduler, either don't set the gpu_count or set it to 0.

>When using the scheduler, gpu_count must match the actual 
> number of GPUs provided to J2O via the gpu_devices. Setting it higher causes 
> the Redis scheduler to create reservation 
> slots for non-existent GPU indices, and containers assigned those indices will 
> have NVIDIA_VISIBLE_DEVICES pointing to GPUs that don't exist — resulting in 
> silent GPU failures inside the container.
{style="warning"}

#### GPU reservation timeout

> The reservation timeout only affects GPU reservation for J2O. GPUs will be 
> accessible by all other processes outside of J2O regardless of the settings 
> done here!
{style="note"}

To prevent an indefinite lock on a GPU by the scheduler when a process does not end as intended, system administrators can set the timeout time of a reservation. By default, there is no timeout. To set a timeout, enter the command with an appropriate amount of time in seconds for your setup, for example:

```bash
omero config set omero.web.jipipe.gpu_lock_time 7200
```
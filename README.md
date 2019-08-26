# RDK Manifests

This is a repository of manifests created to overlay Thunder / WPE related layers on top of the RDK-V existing manifests. Maybe this will merge to RDK-V in the future, or not, who knows. For now read this to get started. 

## Prerequisites

You need to understand what Yocto / OpenEmbedded is, if you don't know what either Yocto or OpenEmbedded means, please google first. Build an RPI image before coming back here.

Make sure you have `repo`, `git` and `build essentials` installed.

You need to have an account on RDK Central, please visit [rdkcentral](http://rdkcentral.com) to register and make sure your PC is setup to authenticate with code.rdkcentral.com through netrc.

## Getting started

1. `repo init -u git@github.com:webplatformforembedded/rdk-manifests.git -b 19.1 --submodules`
2. `repo sync`
3. Init the target build (see below)


### Target initalisation

Typically a target comes with a [BSP layer](https://en.wikipedia.org/wiki/Board_support_package) and you'll need to run setup-environment from the BSP layer to init the build system. For the sake of this tutorial we'll be initialising a Broadcom reference build, however you'd likely want to do `. meta-rdk-{OEM}/setup-environment` for the device you have. 

1. `. meta-rdk-broadcom-generic-rdk/setup-environment-refboard-rdkv`
2. Select Yocto 2.2, this relies on the `morty` branch of OE and WPE layers
3. `bitbake-layers add-layers ../meta-wpe`
4. `bitbake-layers add-layer ../meta-rdk-wpe`

### Build it

To start the build, use [bitbake](https://www.yoctoproject.org/docs/1.6/bitbake-user-manual/bitbake-user-manual.html):

1. bitbake `rdk-thunder-wpe`

**Note**: The `meta-rdk-wpe` layer adds a custom image names `rdk-thunder-wpe`. This builds a pure vanilla Thunder build without any other components. For RDK-V LLC builds that include 3.0/4.0 components please visit their [wiki](https://wiki.rdkcentral.com/display/RDK/RDK-V+%28Raspberry+Pi%29+Build+Instructions+-+Thunderstorm) (e.g. `rdk-generic-hybrid-wpeframework-image`). 

### Switches

The `meta-wpe` and `meta-rdk-wpe` come with a few important switches that are controlled through Distro Features, here's a quick overview:

* Compositor, enables Thunder compositor plugin for initialisation of the compositing layer
* Thunder, enables quick start of Thunder by taking over networking components from the init system
* OpenCDM, enables the opencdm server
* PlayReady (restricted), enables PlayReady DRM
* Widevine (restriteced), enables Widevine DRM

The above features can be enabled/disabled through `DISTRO_FEATURES` append/remove respectively.
For more information please see the `meta-wpe` readme. 

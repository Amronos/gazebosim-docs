# Installing Gazebo with ROS

This document provides guidance on using different versions
of ROS in combination with different versions of Gazebo. We recommend reading
it before installing [ros_gz](https://github.com/gazebosim/ros_gz).

## Picking the "Correct" Versions of ROS & Gazebo

If this is your first time using ROS and Gazebo, and you are not following
specific instructions or tutorials, we recommend using the latest long term
support (LTS) versions of both ROS and Gazebo for the best user
experience. Successive versions of ROS and Gazebo releases are named
alphabetically, and the documentation for each version of ROS and Gazebo will
indicate if it is an LTS version. It is worth noting that each version of ROS
works best with a specific version of each Tier 1 platform. Tier one platforms
are platforms / host operating systems that are used in the development of ROS
and Gazebo. All of this information is outlined in
[REP-2000](https://www.ros.org/reps/rep-2000.html).

To summarize, the best user experience is to use the latest LTS version of ROS
and the Tier 1 platform / operating system recommend for that version of ROS. If
your host operating system does not match the Tier 1 operating system, consider
using the Tier 1 platform in a virtual machine. This approach has the added
benefit of not modifying your host OS and allowing you to roll back your
mistakes.

At the time of writing, our recommendation is that new users install:

* [Ubuntu Noble 24.04](https://www.releases.ubuntu.com/noble/)
* [ROS 2 Jazzy Jalisco](https://www.ros.org/reps/rep-2000.html#jazzy-jalisco-may-2024-may-2029)
* [Gazebo Harmonic](https://gazebosim.org/docs/harmonic/install)

## Summary of Compatible ROS and Gazebo Combinations

This table includes all currently supported versions of ROS and Gazebo. All
other ROS and Gazebo releases are end of life and we do not recommend their
continued use.

|                           | **GZ Fortress (LTS)**   | **GZ Harmonic (LTS)**   | **Gz Ionic**
|---------------------------|-----------------------  | ----------------------  | ------------
| **ROS 2 Rolling**         | ❌                      | ⚡                       | ✅
| **ROS 2 Kilted**          | ❌                      | ⚡                       | ✅
| **ROS 2 Jazzy (LTS)**     | ❌                      | ✅                      | ❌
| **ROS 2 Humble (LTS)**    | ✅                      | ⚡                       | ❌
| **ROS 1 Noetic (LTS)**    | ⚡                       | ❌                      | ❌


* ✅ - Recommended combination
* ❌ - Incompatible / not possible.
* ⚡ - Possible, *but use with caution*. These combinations of ROS and Gazebo can
  be made to work together, but some effort is required.


## Installing the Default Gazebo/ROS Pairing

If you are planning on using a specific version of ROS and don't have a reason
to use a specific version of Gazebo, we recommend using the default version of
Gazebo available from the ROS repository when installing `ros_gz`. You can find 
instructions on adding the ROS repository to your system in the 
[ROS documentation](https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html#add-the-ros-2-apt-repository). 
The following command will install the correct version of Gazebo and `ros_gz` 
for your ROS installation on a Linux system. You should replace `${ROS_DISTRO}` 
with your ROS distribution (e.g. `humble`, `rolling`, `noetic`, etc).

```bash
sudo apt-get install ros-${ROS_DISTRO}-ros-gz
```

This command will install the Gazebo libraries recommended for your ROS
installation.

### ROS 2 Gazebo Vendor packages

As of ROS 2 Jazzy, Gazebo is available from the ROS package repository via
vendor packages. If your package directly depends on a Gazebo library,
instead of relying only on `ros_gz`, refer to
[this documentation](ros2_gz_vendor_pkgs) to learn how to use the
Gazebo vendor packages.

<div class="warning">
The rest of this document provides more information and other, alternative,
installations pathways. We do not recommend the following approaches for new
users.
</div>

## Specific ROS and Gazebo Pairings

Gazebo is an independent project like Boost, Ogre, and many other projects used
by ROS. Usually, the latest version of Gazebo is available at the beginning of
each ROS release cycle (for example Gazebo Fortress for ROS 2 Humble). This
version of Gazebo is selected as the official Gazebo release for that ROS Distro
and is fully integrated, tested, and supported for the life of the ROS
distribution.

Note that all Gazebo libraries follow the [semantic
versioning](http://semver.org/) philosophy for ABI stability. All Gazebo
versions that have the same major number (`gz-sim7_7.0.0`, `gz-sim7_7.1.0`,
`gz-sim7_7.0.1`, ...) are binary compatible and thus interchangeable with a
given ROS distro.

### Installing Gazebo

#### Gazebo Packages for Ubuntu

The easiest way of installing Gazebo on Ubuntu is to use binary packages. There
are two main repositories that host Gazebo simulator and Gazebo libraries: one
is `packages.ros.org` and the other is the Gazebo repository known as
[`packages.osrfoundation.org`](https://packages.osrfoundation.org/gazebo/ubuntu/). At the time
of writing the following packages are available on the following hosts:

 * **packages.ros.org**
   * ROS1 Noetic: Gazebo Citadel
   * ROS2 Humble: Gazebo Fortress
   * ROS2 Jazzy: Gazebo Harmonic (ROS vendor packages)
   * ROS2 Rolling: Gazebo Ionic (ROS vendor packages, changing frequently)

 * **packages.osrfoundation.org**
   * Gazebo Citadel
   * Gazebo Fortress
   * Gazebo Harmonic
   * Gazebo Ionic

This means that including the `osrfoundation.org` repository is not strictly needed
to get the Gazebo binary packages, as it can be installed from the ROS
repository.

## Installing Non-Default Gazebo/ROS 2 Pairings
::: warning
Only use this approach if you absolutely need to run a
version of Gazebo that is not officially supported by your ROS distro. Using
this approach will make it impossible to use the official ROS Ubuntu packages
that depend on Gazebo.

We do not recommend this approach for beginners!
:::

To select a different release of Gazebo than the one officially supported by
your ROS distribution you must either **use non ROS official Gazebo binary packages** or
**manually compile**
[`ros_gz`](https://github.com/gazebosim/ros_gz) from source (this rule applies for
every ROS package that uses a Gazebo library):

  * **Use non ROS official ros_gz binary packages:** `packages.osrfoundation.org` provides
    binary packages for `ros_gz` for some combinations of Gazebo and ROS that are different
    from the officially supported combination. In some circumstances the non-official packages can be outdated or
    even buggy, the use of them is reserved for advanced users.

  * **Manually compile ros_gz:** some combinations of Gazebo and ROS can be prepared to be
    built together but have no binary packages, neither in `packages.ros.org` or `packages.osrfoundation.org`

Both approaches may also require that you modify your ROS or Gazebo source code to support this compilation.

### 📦 Gazebo Harmonic with ROS 2 Humble or Rolling (Use with caution)

Gazebo Harmonic can be used with ROS 2 Humble and non ROS official binary packages hosted
in `packages.osrfoundation.org`. These packages conflict with `ros-humble-ros-gz*`
packages (Humble officially supports Gazebo Fortress).

To install the binary Gazebo Harmonic/ROS 2 Humble packages:

 * Follow [these instruction to install gz-harmonic](https://gazebosim.org/docs/harmonic/install_ubuntu#binary-installation-on-ubuntu)
   from [`packages.osrfoundation.org`](https://packages.osrfoundation.org/gazebo/ubuntu/)
   repository.
 * Install `ros_gz` from the non official binary packages from apt:
   * `apt-get install ros-humble-ros-gzharmonic`

Gazebo Harmonic can be used with ROS 2 Rolling but
[`ros_gz`](https://github.com/gazebosim/ros_gz) will need to be compiled
from source.

 * Follow [these instruction to install gz-harmonic](https://gazebosim.org/docs/harmonic/install_ubuntu#binary-installation-on-ubuntu)
   from [`packages.osrfoundation.org`](https://packages.osrfoundation.org/gazebo/ubuntu/)
   repository.
 * Install [rosdep rules for Gazebo Harmonic](https://github.com/osrf/osrf-rosdep#installing-rosdep-rules-to-resolve-gazebo-harmonic-libraries)
 * Follow the instructions to compile `ros_gz` from source in a colcon workspace
   * [ROS 2 Rolling](https://github.com/gazebosim/ros_gz/tree/ros2#from-source)
     * Be sure of using `export GZ_VERSION=harmonic`

### 📦 Gazebo Fortress with ROS 2 Galactic or ROS 1 Noetic  (Not Recommended)

Gazebo Fortress can be used with ROS 2 Galactic and ROS 1 Noetic by compiling
[`ros_gz`](https://github.com/gazebosim/ros_gz) from source
source.

 * Follow [these instruction to install gz-fortress](https://gazebosim.org/docs/fortress/install_ubuntu#binary-installation-on-ubuntu)
   from [`packages.osrfoundation.org`](https://packages.osrfoundation.org/gazebo/ubuntu/)
   repository.
 * Follow the instructions to compile `ros_gz` from source in a colcon workspace
   * [ROS 2 Galactic](https://github.com/gazebosim/ros_gz/tree/galactic#from-source)
     * Be sure of using `export GZ_VERSION=fortress`
   * [ROS1 Noetic](https://github.com/gazebosim/ros_gz/tree/noetic#from-source)
     * Be sure of using `export GZ_VERSION=fortress`

## Using the Latest Gazebo Source Code for a Gazebo Distribution

The Gazebo team usually backports and releases new versions of each of the
supported Gazebo releases and libraries (i.e: bumping `gz-sim 7.0.0` to gz-sim
`7.1.0`). These updates are hosted first in the `packages.osrfoundation.org`
repository. The ROS repository syncs from the `osrfoundation.org` repository
frequently but versions can be different.

Getting the latest versions of the Gazebo libraries and simulator is as easy
as installing the [`osrfoundation.org` repository](install_ubuntu_src.md#install-dependencies)
together with the ROS repository. Updates should be fully compatible.

### FAQ

#### I am not using ROS at all, which version should I use?

If you don't need ROS support, the recommended version is the latest released
version that can be [installed using the `osrfoundation.org` repo](https://gazebosim.org/docs)
depending on your platform.

#### I want to use Gazebo Harmonic with ROS 2. Where are the packages?

We provide binary packages for `ros_gz` for the following Gazebo and ROS 2 pairings in the packages.osrfoundation.org repository
but these packages are not official ROS packages, which means if there is a breaking change in the corresponding ROS 2 distribution,
there will be a short delay before these packages are rebuilt with the changes. Installing these packages may also cause conflicts with
other ROS packages that depend on Gazebo-classic.


|                         | **Gazebo Harmonic (LTS)**   |
|------------------------ | --------------------------  |
| **ROS 2 Humble (LTS)**  | `ros-humble-ros-gzharmonic` |


#### What is the difference between a ROS vendor package and other packages listed in this  page?

The ROS vendor packages are (indeed) ROS packages (built by the ROS buildfarm and hosted in the ROS repository) that provide the
installation of the Gazebo librariess. In this page, unless mentioned the vendor in the name, the rest of packages are standard
debian packages the ones installed by the package manager (usually apt).

#### Where I can find the different features implemented on each Gazebo version?

The best place is the Gazebo web page that hosts a list of the
[main features available in each Gazebo distribution](release-features).

Some notes are regularly posted on the [Gazebo community
site](https://community.gazebosim.org/tags/c/release-announcements-and-discussions/10/release)
and special posts and videos are also posted there when a new release is out:
See the one for
[Garden](https://community.gazebosim.org/t/gazebo-garden-release/1627) or the
one for
[Fortress](https://community.gazebosim.org/t/ignition-fortress-release/1127) as
examples.

Additionally, when navigating through the different versions of Gazebo there is a menu item
called "[Feature Comparison](comparison)" which provides a comparison with the features of Gazebo-Classic.
In the top right corner of the [Gazebo documentation page](https://gazebosim.org/docs) the specific Gazebo version to compare with can be selected.

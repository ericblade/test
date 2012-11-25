How to Build on Linux
=====================

## Building using OpenEmbedded 

Using the meta-webos layer for OpenEmbedded is the preferred method of building Open webOS components.
Usually, this is what you will want to do, so that your package can be installed into an Open webOS system, or as part of an Open webOS image.

### Building the latest "stable" version

Clone the repository at http://www.github.com/openwebos/build-webos and follow the instructions in that README to build Open webOS.

To build or rebuild a single Open webOS component, if your build-webos directory is ~/openwebos/build-webos, and you are wanting to rebuild the component called "luna-universalsearchmgr", do:

    $ cd ~/openwebos/build-webos
    $ make cleanall-luna-universalsearchmgr luna-universalsearchmgr

The resulting IPK package will be in your BUILD-[target-machine] directory, under deploy/ipk/[architecture], such as this example:

    ~/openwebos/build-webos/BUILD-qemux86/deploy/ipk/i586/luna-universalsearchmgr_2.0.0-1.00-r5_i586.ipk

You can transfer this to your existing image, and install it by logging into the Open webOS system, and using:

    $ ipkg install /path/to/luna-universalsearchmgr_2.0.0-1.00-r5_i586.ipk

Or you can create a completely new Open webOS image with:

    $ make webos-image

### Building your local clone

After successfully building the latest stable version, you may configure build-webos to build this component from your own local clone.

You can specify what directory to use as the local source inside the file "global-webos.conf" in your home directory, or within the file "webos-local.conf" within the build-webos directory, by adding the following:

    S_pn-[component-name] = "/path/to/component/source"

such as in this example:

    S_pn-luna-universalsearchmgr = "/home/user/openwebos/luna-universalsearchmgr"

Then follow the instructions above to rebuild and install this package.

## Building for Open webOS Desktop

It is often desireable, for rapid iteration and testing purposes, to build a component for use within the Open webOS Desktop system.

### Building the latest "stable" version

Clone the repository at http://www.github.com/openwebos/build-desktop and follow the instructions in the README file.

### Building your local clone

First, follow the directions to build the latest "stable" version.

To build your local clone of a component, such as luna-sysmgr, instead of the "stable" version installed with the build-webos-desktop script:

* Open the build-webos-desktop.sh script with a text editor
* Locate the function build_component-name (i.e., build_luna-sysmgr)
* Change the line "cd $BASE/luna-sysmgr" to use the folder containing your clone, for example "cd ~/github/luna-sysmgr"
* Close the text editor
* Remove the file ~/luna-desktop-binaries/component-name/luna-desktop-build.stamp (~/luna-desktop-binaries/luna-sysmgr/luna-desktop-build.stamp)
* Rebuild by running the build-webos-desktop.sh script again

Cautions:

* When you re-clone openwebos/build-desktop, you'll have to overwrite your changes and reapply them
* Components often advance in parallel with each other, so be prepared to keep your cloned repositories updated
* Fetch and rebase frequently

## Building Standalone (without webOS)

This component of webOS can be built as a standalone piece that does not depend upon the rest of the system. 

### Dependencies

Below are the tools and libraries (and their minimum versions) required to build this package:

* insert list
* of dependencies
* here
* usually will include
* openwebos/cmake-modules-webos 1.0.0 RC2
* and cmake (version required by openwebos/cmake-modules-webos)
* as well as others

### Building Standalone

#### Using cmake

Once you have downloaded the source, execute the following to build it (after changing into the directory under which it was downloaded):

    $ mkdir BUILD
    $ cd BUILD
    $ cmake ..
    $ make
    $ sudo make install

The directory under which the files are installed defaults to <tt>/usr/local/webos</tt>.
You can install them elsewhere by supplying a value for <tt>WEBOS_INSTALL_ROOT</tt> when invoking <tt>cmake</tt>. 

For example:

    $ cmake -D WEBOS_INSTALL_ROOT:PATH=$HOME/projects/openwebos ..
    $ make
    $ make install

will install the files in subdirectories of <tt>$HOME/projects/openwebos</tt>.

Specifying <tt>WEBOS_INSTALL_ROOT</tt> also causes <tt>pkg-config</tt> to look in that tree first before searching the standard locations.
You can specify additional directories to be searched prior to this one by setting the <tt>PKG_CONFIG_PATH</tt> environment variable.

To see all of the make targets that CMake has generated, issue:

    $ make help
    
#### Using make (not cmake)

First, make sure that you have installed all the required dependencies listed above (excepting cmake and cmake modules).

Next, cd to the folder where you have cloned this source code, and execute the following:

    $ make -f Makefile.Ubuntu.Release install

This will create the binary [mojodb-luna] under the folder [release-linux-x86][, along with libraries libmojocore.so, libmojodb.so and libmojoluna.so]

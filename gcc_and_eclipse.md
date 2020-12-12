# Development with GCC, Eclipse and nRF5x SDK
- Almost copied from [Development with GCC and Eclipse - devzone.nordicsemi.com](https://devzone.nordicsemi.com/nordic/nordic-blog/b/blog/posts/development-with-gcc-and-eclipse)
  > The nRF5x series Software Development Kits (SDK) come with Makefiles for use with the GNU ARM toolchain.
- I used Ubuntu 20.04, but with minimal changes are needed to use this configuration on Windows.
## Requirements
- GNU toolchain for ARM Cortex-M (compiler and GDB debugger)
  - [Download link](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)
  - Download and install latest version
  - Make sure to add the path to your toolchain to your OS PATH environment variable.
    I added this to `~/.bashrc`: `export PATH=$PATH:/home/bzgec/gcc-arm-none-eabi/bin` (this is
    the folder where executables are)
  - Verify that the PATH is set correctly: `arm-none-eabi-gcc --version`
- GNU make
  - > Linux and OS X already have the necessary shell commands, but GNU make may not be a part of
    > the standard distro. Call `make -v` from the terminal to check whether it is installed or not.
    >
    > On Linux it may be different ways to obtain GNU make depending on your distro, if not
    > installed already. On Ubuntu you can get by entering this command:
    > `sudo apt-get install build-essential checkinstall`
  - > On windows it can be obtained by installing the *GNU ARM Eclipse Windows Build Tools*
    > package from the [GNU ARM Eclipse plug-in project](https://xpack.github.io/windows-build-tools/).
- Nordic nRF5x SDK
  - [Download SDK](https://www.nordicsemi.com/Software-and-Tools/Software/nRF5-SDK/Download).
    I tested with 17.0.2
## Check if everything works as it should
1. > To build an example in the SDK you first need to set the toolchain path in `makefile.windows`
   > or `makefile.posix` depending on platform you are using. That is, the `.posix` should be
   > edited if your are working on either Linux or OS X. These files are located in:
   > `<SDK>/components/toolchain/gcc`
   > Open the file in a text editor, and make sure that the `GNU_INSTALL_ROOT` variable is
   > pointing to your Gnu tools for ARM embedded Processors install directory.
   > Correct values for my current setup:
   > ```
   > GNU_INSTALL_ROOT ?= /home/bzgec/gcc-arm-none-eabi/bin/
   > GNU_VERSION ?= 9.3.1
   > GNU_PREFIX ?= arm-none-eabi
   > ```
2. > Now you can try to build one of the example projects. Will use the blinky example here to keep it simple:
   > Open terminal and change directory to: `<SDK>/examples/peripheral/blinky/<board name>/blank/armgcc/`
   In my case `<board name>` is `pca10040` for `nRF52832`.
   > Type `make`. GNU Make should start the build using the Makefile and output the result in
   > the `_build` directory.
   >
   > If you instead get an error saying something like `the sysem cannot find the files specified`
   > it typically means that the GNU toolchain path is set incorrectly.
   > Verify the path in makefile.windows/posix if you get this.
   >
   > Errors like `make: No rule to make target '_build/Nordic', needed by 'nrf51422_xxac`.
   > Stop can also be an indication that build tools are unable to handle whitespaces
   > in the path. E.g, `c:/nordic semicondtor/SDK/...` Workaround is to remove any whitespaces
   > you may have with underscores.
3. To load the FW I used OpenOCD ([link](./openOCD.md) on how to use OpenOCD with
   ST-Link v2 or Raspberry Pi): `openocd -f interface/stlink.cfg -f target/nrf52.cfg -c "init" -c "program _build/nrf52832_xxaa.hex verify reset exit"`
   If you have problems it OpenOCD because of ESP-IDF you can use: `/usr/local/bin/openocd -f /usr/local/share/openocd/scripts/interface/stlink.cfg -f /usr/local/share/openocd/scripts/target/nrf52.cfg -c "init" -c "program _build/nrf52832_xxaa.hex verify reset exit"`

   Another option is to change the `Makefile` of this project to be able to run `make flash` to
   load FW to IC. Change `flash: default`:
   ```
   flash: default
       @echo Flashing: $(OUTPUT_DIRECTORY)/nrf52832_xxaa.hex
       /usr/local/bin/openocd -f ~/openocd/tcl/interface/stlink.cfg -f ~/openocd/tcl/target/nrf52.cfg -c "init" -c "program $(OUTPUT_DIRECTORY)/nrf52832_xxaa.hex verify reset exit"
	   # nrfjprog -f nrf52 --program $(OUTPUT_DIRECTORY)/nrf52832_xxaa.hex --sectorerase
	   # nrfjprog -f nrf52 --reset
   ```
## Setting up Eclipse the first time

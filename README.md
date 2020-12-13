# nRF52 programming notes
- This are just my notes on how to program/debug nRF52 ICs
- ST-Link v2 with [openOCD](https://sourceforge.net/p/openocd/code/ci/master/tree/)
  *CAN* be used to program nRF5 ICs
- Some nRF5 ICs come with flash **AP lock - Access port protection**, which sadly
  can't be unlocked with ST-Link - you can use Raspberry Pi or
  [Black Magic Probe](https://github.com/blacksphere/blackmagic) (not tested with BMP but it should
  work)
- [Configure OpenOCD, flash .hex files, unlock AP protection](./openOCD.md)

## Nordic SoftDevice
- *SoftDevice* is the name of the *Nordic Bluetooth protocol stack* (plus ANT protocol stack in
  some cases).
- Each version of *nRF5 SDK* is released together with the tested and working *SoftDevice*.
- Composed of:
  - Bluetooth LE:
    - Central (master)
    - Peripheral (slave)
  - ANT
- Naming rule:
  - `Sxyz`
    - `x`: Type of protocol (1 - Bluetooth LE, 2 - ANT, 3 - combined Bluetooth LE and ANT)
    - `y`: Bluetooth LE role (1 - slave, 2 - master, 3 - combined master and slave, 4 - combined
           feature rich stack)
    - `z`: SoC family (0 - nRF51, 2 or 3 - nRF52)
  - Always check if desired IC is supported by *SoftDevice*

### References
- [devzone.nordicsemi.com](https://devzone.nordicsemi.com/nordic/short-range-guides/b/getting-started/posts/introduction-to-nordic-nrf5-sdk-and-softdevice)
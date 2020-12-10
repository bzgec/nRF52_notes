# nRF52 programming notes
- This are just my notes on how to program/debug nRF52 ICs
- ST-Link v2 with [openOCD](https://sourceforge.net/p/openocd/code/ci/master/tree/)
  *CAN* be used to program nRF5 ICs
- Some nRF5 ICs come with flash **AP lock - Access port protection**, which sadly
  can't be unlocked with ST-Link - you can use Raspberry Pi or
  [Black Magic Probe](https://github.com/blacksphere/blackmagic) (not tested with BMP but it should
  work)
- [Configure OpenOCD, flash .hex files, unlock AP protection](./openOCD.md)
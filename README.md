# eMMC-Reader
A GL3224-based eMMC reader

## Why?

The Sabrent USB readers that had this chip were recently (2022?) updated to use the 48-QFN version with dual LUNs. This happened to break the single-bit eMMC mode, and that makes me very sad. Rather than trawling eBay for NOS packaging, I figured I should just make a better tool.

This also includes a TXS02612 that is being used as a 1.8V level shifter like on an Exploitee.rs [Low Voltage eMMC Adapter](https://shop.exploitee.rs/shop/p/low-voltage-emmc-adapter) because those are great.\
Note that this is a mux, which isn't necessary in this application. I could wire it up to an SD card socket, but that's overkill. According to [TI's Product Overview for SDIO voltage translators](https://www.ti.com/lit/an/scea096/scea096.pdf), the TXS0206 is a non-mux version of this, but it only supports 60Mbps (as opposed to 120Mbps) and is a WCSP. I've gotten more than 60Mbps out of an exploitee.rs board in single-bit mode, so I'm going to stick with the proven chip.


#### Why not USB-C?
Because I don't trust you not to break it...

The GL3224 supports USB 3.0 (5Gbps). To implement that with a USB-C connector, there are two options:
1. Use a plug and hook up one pair of `SS[R,T]X` lines as well as a 5K1 pulldown on CC1. The computer into which this is plugged will have to figure out which pair is live.
2. Use a receptacle and include a SuperSpeed-capable mux like an `HD3SS3220` since you can't trust a user to plug the cable in the "right" way.

USB-C plugs suck because they need thin PCBs and you will break one off in your computer at some point. Unless you use a (technically illegal) extension cable, and then we're back to the same impasse as we had with the receptacle in the first place. It's easier and more robust to use a USB 3.0 Type-A connector (I typically wind up using the Sabrent USB reader with a USB 2.0 extension cable anyway to keep the clock speeds down).

## Links

- [Somebody who reversed a GL3224 adapter](https://www.801labs.org/research-portal/post/reverse-engineering-4-layer-pcb/)
- [Sample (reverse-engineered as above) schematic](https://github.com/dvdfreitag/USB3-eMMC)
- [Eagle Library for the 48- and 32-QFN parts](https://github.com/dvdfreitag/eagle_libraries/blob/bc423563c25ef42febea000f4a0536ccb97d0efd/GenesysLogic.lbr)

Note that this schematic is a little more grand than the bare minimum necessary; the Sabrent reader that inspired this project doesn't bother with the external oscillator.


# mailbox

Firmware for the RP2040 or RP2350 that sits in a retro machine's cartridge slot or expansion
bus and relays traffic to a FujiNet over USB CDC. The console sees a block of memory it can
read, and sometimes write: the FujiBus mailbox. [common/](common/) carries what every cart
shares; each other directory is one console.

The MSX and CoCo controller in the repository root is a different design: it emulates a ROM
chip and has its own build.

## Layout
| Directory | Target |
|---|---|
| [common/](common/) | FujiBus codec, USB transport, mailbox service, push store, build helpers and host tests shared by the carts |
| [template/](template/) | the cart a new bring-up starts from: an 8K ROM window on an RP2354B, run by no console |
| [tools/](tools/) | `cmpfw.py` builds the carts at two git refs and names the functions whose code differs |
| [studio2/](studio2/) | RCA Studio II cartridge on an RP2354B; serves the CDP1861 display raster itself |
| [arcadia/](arcadia/) | Emerson Arcadia 2001 cartridge on an RP2040; 2650 clients |
| [astrocade/](astrocade/) | Bally Astrocade cartridge on an RP2040 `fujicade` board; Z80 clients |
| [channelf/](channelf/) | Fairchild Channel F Videocart on an RP2040 `fujichannelf` board; F8 clients |
| [nes/](nes/) | NES cartridge on an RP2354B with two 512K SRAMs for PRG and CHR; 6502 clients |
| [sms/](sms/) | Sega Master System cartridge on an RP2354B with 1 MB of SRAM; Z80 clients |
| [atari-7800/](atari-7800/) | Atari 7800 cartridge on an RP2354B with 512K of SRAM; 6502 clients |
| [atari-5200/](atari-5200/) | Atari 5200 cartridge on an RP2354B serving the window itself, behind a 74HCT541; 6502 clients |
| [s100/](s100/) | S-100 card (Altair, IMSAI) on an RP2354B: a FujiBus pipe, an Altair FDC+ whose drives are FujiNet slots, an 88-2SIO and a boot page; 8080 clients |

## How it fits
- A cart's `build-cart.sh` sets its board, firmware name and client, then sources
  [common/](common/)'s `scripts/build-cart.lib.sh`. `--baked` builds from the client committed
  under `firmware/baked/`, with none of the console's toolchain.
- Every cart's `firmware/host_test/` includes [common/](common/)'s `host_test/cart.mk`.
- The FujiNet side is `lib/bus/rs232` in [fujinet-firmware](https://github.com/FujiNetWIFI/fujinet-firmware).

## Build
`make host-tests` needs only a C compiler and Python. `make <cart>` needs an ARM GCC toolchain
with newlib, CMake, Ninja and a pico-sdk 2.3.0 checkout in `PICO_SDK_PATH`.

## Notes
Licensed GPLv3, see `LICENSE`; a cart that vendors other code says so in its own
`PROVENANCE.md` or `LICENSE`.

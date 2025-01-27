# FdtBusPkg - Devicetree-based Platform Device Driver Development for Tiano UEFI.

This repo implements support for developing Tiano platform device drivers
compliant to the UEFI Driver Model, by performing driver binding and
configuration using a Devicetree. Such a Devicetree is typically
either passed to UEFI by higher-privileged firmware.

> [!NOTE]
> Relation to https://github.com/intel/FdtBusPkg - this is a direct continuation
> of the work I've done at Intel before I left. Most likely, the Intel repo is dead,
> as I was the owner and the only remaining contributor to the project.

Advantages:
- Allows UEFI developers to fully embrace modularity and code reuse.
- Facilitates development of complex (graphics, NIC, etc) drivers.
- Enables a single firmware binary to work across SoC revisions and
  board designs.

FdtBusPkg consists of FdtBusDxe, a bus driver, and a number
of examples drivers and libraries for demoing with the RISC-V
OVMF firmware. FdtBusDxe is responsible for enumerating
DT controllers based on Devicetree nodes, and implementing
`EFI_DT_IO_PROTOCOL` for basic operations on such controllers, such as
device property access, register I/O, DMA buffer handling and child
device enumeration.

See further documentation:
- [FdtBusPkg Documentation Style and Terms Definitions](Docs/StyleAndTerms.md)
- [Devicetree Device Drivers](Docs/DeviceDrivers.md)
- [EFI Devicetree I/O Protocol](Docs/DtIoProtocol.md)
- [EFI Devicetree Interrupt Protocol](Docs/DtInterruptProtocol.md)
- [FdtBusPkg-Specific Devicetree Bindings](Docs/DtBindings.md)
- [FdtBusDxe Overview](Docs/FdtBusDxe.md)
- [Another README for Developers](Docs/Developers.md)

FdtBusPkg components can be used on any architecture, but have been
developed and tested with RISC-V. They should be reusable out of the box
on AArch64 platforms as well, barring any missing dependencies.

Note: this is Devicetree being used internally by UEFI. There is no
relation to using Devicetree as possible mechanism of describing
hardware configuration to an OS.

See the [presentation video](https://www.youtube.com/watch?v=2w9iQE8jA1w) and [slides](Docs/Uefi2023/slides.pdf) from the UEFI Fall 2023 Developers Conference and Plugfest. Also see the [short demo video published February, 2024](https://youtu.be/9RqKq4wGYZI).

## Updates

| When | What |
| :-: | ------------ |
| October 2024 | Support indirect access to preconfigured BARs. |
| September 2024 | Range translation, DMA range narrowing, reg-attrs, range-attrs, improved address-cells and size-cells handling, PciHostBridgeFdtDxe support for indirect configuration space access, legacy VGA ranges, preconfigured BARs. PciInfo tool. |
| August 2024 | Various fixes, DtInterrupt protocol. |
| February 2024 | Docs complete. DtInfo, DtProp and DtReg tools added. VirtNorFlashDxe, PciSioSerialDxe, PciHostBridgeFdtDxe drivers ported. Demo video at https://youtu.be/9RqKq4wGYZI. |
| January 2024 | Open sourced. Work on documentation. |
| October 2023 | Presented at the UEFI Fall 2023 Developers Conference and Plugfest. See the [presentation slides](Docs/Uefi2023/slides.pdf). |
| 2023 | Reported to RISE as a 2024 priority. |

## Quick Start

To build RISC-V OVMF firmware enabled with FdtBusPkg components:

        $ git clone https://github.com/tianocore/edk2.git
        $ cd edk2
        $ git submodule add https://github.com/intel/FdtBusPkg
        $ git submodule update --init --recursive
        $ . edksetup.sh
        $ git am FdtBusPkg/Docs/edk2-patches/*
        $ git am FdtBusPkg/Docs/ovmf-patches/*
        $ export GCC_RISCV64_PREFIX=... (if you are on a non-RISCV64 system)
        $ build -a RISCV64  -p OvmfPkg/RiscVVirt/RiscVVirtQemu.dsc -t GCC -b DEBUG

See [the README for Developers](Docs/Developers.md) for more directions.

## License

FdtBusPkg is licensed under the BSD-2-Clause-Patent license (just like Tiano).

## Security Policy

I am committed to addressing security vulnerabilities with a reasonable response time for an open-source/hobby project and providing clear guidance on the solution, impact, severity and mitigation.

### Reporting a Vulnerability

If you have discovered potential security vulnerability, please validate with the latest commit in the repo. If the issue reproduces - raise an issue. It is important to include the following details:
- Detailed description of the vulnerability, with a repro if possible.
- Information on known exploits.

## Contribute

Contributions are welcome. Please raise issues and pull requests.

Please see the [policy on contributions](CONTRIBUTING.md) and the [Code of Conduct](CODE_OF_CONDUCT.md).

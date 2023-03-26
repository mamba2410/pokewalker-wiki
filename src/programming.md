# Programming the Pokewalker

## Useful resources

- [E8a Emulator](https://www.renesas.com/us/en/software-tool/e8a?doc_order=field_document_revision_date&doc_sort=asc#additional_details)
- [E8a Emulator Users Manual](https://www.renesas.com/us/en/document/mat/e8a-emulator-users-manual?r=1169666)
- [Renesas Emulator Comparison](https://www.renesas.com/us/en/document/mat/chip-debuggers-performance-property?r=1169666)
- [Notes on Connecting the H8/38606F](https://www.renesas.com/us/en/document/mat/e8a-emulator-additional-document-users-manual-notes-connecting-h8300h-super-low-power-series?r=1169666)
- [Renesas Flash Development Toolkit Available MCUs](https://www.renesas.com/us/en/document/oth/available-mcus-flash-development-toolkit?r=1169886) (page 13)
- [H8/38602R Hardware Manual](https://www.renesas.com/us/en/document/mah/h838602r-group-hardware-manual) (See Section 6.4 - Flash Memory Programming and Erasure)

The E8a Emulator uses NMIB, Test, E7_0, E7_1, E7_2, RESB pins to debug and program.

H8/38602R Group Hardware Manual Section 6.3.1 on boot mode programming:
Allows programming over the UART. MCU sends one byte of 0x00 to indicate it's ready to rx data.
Host then sends 0x55 as an ack.
Host sends a programming control program to the MCU addr (0xFB80-0xFF7F on a H8/38602R), control then goes to this program.
Serial comms are still up but general regs and SP need to be initialised by program.
To exit, hold reset, set NMI, release reset.
Test and NMI need to be constant during the operation.

For a flowchart, see page 107 (PDF page 141).


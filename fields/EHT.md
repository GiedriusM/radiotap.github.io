---
title: EHT
categories: [suggested]
---
Type
: 34

Structure
: - u32 known
  - u32 data[6]
  - u32 user_info[] (Note this is variable length)

Required Alignment
: 4

Unit(s)
: none

## known

Note that for trigger-based (TB) PPDUs, the EHT-SIG isn't present over the
air as all the information for each station is provided in the trigger
frame. Sniffer should - where possible - provide a subset of the OFDMA
related fields to simplify understanding of the resulting capture (i.e. to
be able to read it without having to go back to the prior trigger frame.)

| **bits**         | **OFDMA (including TB)** | **MU-MIMO** | **EHT sounding** |
| **`0x00000001`** | Content Channel Index Known | (same) | (same) |
| **`0x00000002`** | Spatial Reuse Known | (same) | (same) |
| **`0x00000004`** | Guard Interval Known | (same) | (same) |
| **`0x00000008`** | LTF Known | (same) | (same) |
| **`0x00000010`** | EHT-LTF Known | (same) | (same) |
| **`0x00000020`** | LDPC Extra Symbol Segment Known | (same) | (reserved) |
| **`0x00000040`** | Pre-FEC Padding Factor Known | (same) | (reserved) |
| **`0x00000080`** | PE Disambiguity Known | (same) | (reserved) |
| **`0x00000100`** | Disregard Known | (same) | (reserved) |
| **`0x00000200`** | (reserved) | (reserved) | Disregard Known |
| **`0x00001c00`** | # of known RU Allocations (TBD) | (reserved) | (reserved) |
| **`0x00002000`** | CRC1 Known | (same) | (same) |
| **`0x00004000`** | Tail1 Known | (same) | (same) |
| **`0x00008000`** | CRC2 Known | (reserved) | (reserved) |
| **`0x00010000`** | Tail2 Known | (reserved) | (reserved) |
| **`0x00020000`** | (reserved) | (reserved) | NSS Known |
| **`0x00040000`** | (reserved) | (reserved) | Beamformed Known |
| **`0x00080000`** | (reserved) | Number Of Non-OFDMA Users Known | (reserved) |
| **`0x00100000`** | (reserved) | User Encoding Block CRC Known | (reserved) |
| **`0x00200000`** | (reserved) | User Encoding Block Tail Known | (reserved) |
| **`0x00400000`** | RU/MRU Size Known | (same) | (reserved) |
| **`0x00800000`** | RU/MRU Index Known | (same) | (reserved) |
| **`0x01000000`** | TB RU Allocation Known | (same) | (reserved) |
| **`0xfe000000`** | (reserved) | (reserved) | (reserved) |

Note: The "RU/MRU Size" and "RU/MRU Index" fields are provided for sniffers
that cannot provide the entirety of the RU allocations and user information
fields (which would allow knowing exactly which AID got which RU allocation),
but can only provide a subset of the information. They apply only to the
captured AID. It's advantageous to provide this even when all the RU
Allocaction and User Information fields are known, so the display tool need
not parse all of them.

Some values here could also appear for non-OFDMA PPDUs.

## data[0]

| **bits**         | **meaning** |
| **`0x00000003`** | Content Channel Index (numbered by first appearance in order of sub-carrier index) |
| **`0x00000078`** | Spatial Reuse |
| **`0x00000180`** | GI |
| **`0x00000600`** | LTF |
| **`0x00003800`** | EHT-LTF |
| **`0x00004000`** | LDPC extra symbol segment |
| **`0x00018000`** | Pre-FEC padding factor |
| **`0x00020000`** | PE Disambiguity |
| **`0x000c0000`** | Disregard (EHT sounding) |
| **`0x00300000`** | (reserved) (EHT sounding) |
| **`0x003c0000`** | Disregard (non EHT sounding) |
| **`0x03c00000`** | CRC1 (OFDMA: B0-16+9N, EHT sounding: B0-15, non-OFDMA: B0-41) |
| **`0xfc000000`** | Tail1 |

## data[1]

| **bits** | **meaning** |
| **`0x0000001f`** | RU/MRU Size (0: 26, 1: 52, 2: 106, 3: 242, 4: 484, 5: 996, 6: 2x996, 7: 4x996, 8: 52+26, 9: 106+26, 10: 484+242, 11: 996+484, 12: 996+484+242, 13: 2x996+484, 14: 3x996, 15: 3x996+484) |
| **`0x00000020`** | (reserved) |
| **`0x00001fe0`** | RU/MRU Index (see IEEE 802.11be Draft 1.3 section 36.3.2 "Subcarrier and resource allocation") |
| **`0x003fe000`** | RU Allocation-1::1 |
| **`0x7fc00000`** | RU Allocation-1::2 |
| **`0x80000000`** | (reserved) |

Note: The RU/MRU Size and RU/MRU Index are calculated fields, ideally the
sniffer should provide them for all OFDMA and punctured non-OFDMA PPDUs to
simplify filtering and other data uses.

## data[2]

| **bits** | **meaning** |
| **`0x000001ff`** | RU Allocation-2::1 |
| **`0x0003fe00`** | RU Allocation-2::2 |
| **`0x07fc0000`** | RU Allocation-2::3 |
| **`0xf8000000`** | (reserved) |

## data[3]

| **bits** | **meaning** |
| **`0x000001ff`** | RU Allocation-2::4 |
| **`0x0003fe00`** | RU Allocation-2::5 |
| **`0x07fc0000`** | RU Allocation-2::6 |
| **`0xf8000000`** | (reserved) |

## data[4]

| **bits** | **meaning** |
| **`0x0000000f`** | CRC2 (OFDMA only: for RU Allocation-2) |
| **`0x000003f0`** | Tail2 (OFDMA only: after RU Allocation-2) |
| **`0x00000c00`** | (reserved) |
| **`0x0000f000`** | NSS (EHT sounding) |
| **`0x00010000`** | Beamformed (EHT sounding) |
| **`0x000e0000`** | Number Of Non-OFDMA Users |
| **`0x00f00000`** | User Encoding Block CRC |
| **`0x3f000000`** | User Encoding Block Tail |
| **`0xc0000000`** | (reserved) |

## data[5]

| **bits** | **meaning** |
| **`0x00000001`** | TB RU Allocation: PS 160 |
| **`0x00000002`** | TB RU Allocation: PS 80 |
| **`0x000001fc`** | TB RU Allocation: B7--B1 |
| **`0xfffffe00`** | (reserved) |

Note: the `0x1ff` bits here indicate the RU Allocation for the captured
station (AID) in the trigger-based (TB) format, per Table 9-53a "Encoding of
PS160 and RU Allocation subfields in an EHT variant User Info field". This
may be given by the sniffer even for downlink OFDMA PPDUs if calculated from
the RU Allocation from EHT-SIG for the given station. Note the bit split to
ease cross-referencing Table 9-53a.

## `user_info` entry

Each `user_info` entry carries information for a given user entry
in the EHT preamble. If multiple user fields are captured, ideally
all of them are, and they should be given in the same order as they
were ordered in the frame.

Note that to simplify parsing we indicate OFDMA and MU-MIMO information
via separate known bits, so that parsers can show these fields without
consulting other information on whether it was an OFDMA or MU-MIMO PPDU.

The "Data captured for this user" bit indicates which user the data was
captured for that this radiotap header is attached to. It should be set
for exactly one `user_info` entry in the entire radiotap header, even if
potentially the entire EHT field is given multiple times.

| **bits**         | **OFDMA** | **MU-MIMO** |
| **`0x00000001`** | STA-ID known | (same) |
| **`0x00000002`** | MCS known | (same) |
| **`0x00000004`** | Coding known | (same) |
| **`0x00000008`** | Reserved known | (reserved) |
| **`0x00000010`** | NSS known | (reserved) |
| **`0x00000020`** | Beamforming known | (reserved) |
| **`0x00000040`** | (reserved) | Spatial Configuration known |
| **`0x00000080`** | Data captured for this user | (same) |
| **`0x0007ff00`** | STA-ID | (same) |
| **`0x00080000`** | Coding | (same) |
| **`0x00f00000`** | MCS | (same) |
| **`0x0f000000`** | NSS | (overlap) |
| **`0x10000000`** | Reserved | (overlap) |
| **`0x20000000`** | Beamforming | (overlap) |
| **`0x3f000000`** | (overlap) | Spatial Configuration |
| **`0xc0000000`** | (reserved) | (reserved) |
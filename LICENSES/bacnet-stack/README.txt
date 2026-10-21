bacnet-stack in the SlateTap firmware

Upstream:  https://github.com/bacnet-stack/bacnet-stack (the tag and commit are in FILES.txt)
Compiled:  the files listed in FILES.txt, each with the license its header names
Changed:   one file, src/bacnet/datalink/bsc/bvlc-sc.c, by bvlc-sc-byte-order.patch (this folder), applied to a copy
           of the file at build time; no other stack file is modified

The patch: bacnet-stack 1.6.1 copies every 2-octet BVLC-SC field (Message ID, Error Class, Error Code, Maximum BVLC
Length, Maximum NPDU Length, the header option lengths) in host byte order, which is least significant octet first
on the ESP32-S3. ASHRAE 135 Annex AB, bacpypes3 and Wireshark send and read them most significant octet first. The
patch uses the stack's own encode_unsigned16() and decode_unsigned16() for those fields.

To rebuild the stack's part of the firmware from source: check out the tag, apply the patch to bvlc-sc.c (for
example "git apply bvlc-sc-byte-order.patch" from the stack's root), and compile the files of FILES.txt.

License texts: license-readme.txt (upstream's license/readme.txt, its note on the GPL with the GCC linking
exception), GPL-2.0, GCC-exception-2.0, MIT, BSD-3-CLAUSE. Not a legal reading: see ../README.md.

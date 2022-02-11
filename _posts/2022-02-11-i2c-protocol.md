---
layout: post
title: I2C Protocol
author: juanrgar
---

[I2C](https://en.wikipedia.org/wiki/I%C2%B2C) is a synchronous, serial
communication bus designed for connecting devices at short distance, i.e.,
intra-board. It was invented by Philips in 1982, and it's still widely used in
simple peripherals. [SMBus](https://en.wikipedia.org/wiki/System_Management_Bus)
is derived from I2C and specifically designed for peripherals typically found
on a motherboard.

## Physical Layer

I2C consists of various nodes that can either be controllers or peripherals. It
allows multiple controllers in the same network, and the specification
describes an arbitration mechanism. Nodes are connected using 2 lines, SCL and
SDA: SCL carries the clock signal (as previously stated, it's synchronous),
while SDA carries the data bits (i.e., half-duplex).

The bus design supports various maximum speeds:

| Mode                 | Maximum speed |
| -------------------- | ------------- |
| Standard mode (Sm)   | 100 kbit/s    |
| Fast mode (Fm)       | 400 kbit/s    |
| Fast mode plus (Fm+) | 1 Mbit/s      |
| High-speed mode (Hs) | 1.7 Mbit/s    |
| High-speed mode (Hs) | 3.4 Mbit/s    |

Please note that the actual amount of transferred data varies depending on the
messages flow, as there's some overhead in the protocol.

## Protocol

First of all, I2C defines a 7-bit address space, where each peripheral has a
unique address. The full 7-bit space is not usable, though, because there are
some addresses reserved to extend the address space to 10 bits.

A controller initiates a data transfer using a START condition, and can
terminate it using a STOP condition. A START condition is indicated by a
high-to-low transition of SDA with SCL high; a STOP condition, on the other
hand, is indicated by a low-to-high transition of SDA with SCL high.

After the START condition, it comes the device selector or address byte,
which consists a 7-bit address followed by a R/<span
style="text-decoration:overline">W</span> bit. The addressed peripheral, if
any, responds with a <span style="text-decoration:overline">ACK</span>/NACK
bit.

The R/<span style="text-decoration:overline">W</span> bit determines what comes
next. If the controller signaled a read operation, then the peripheral begins
sending data bytes; each byte is followed by an <span
style="text-decoration:overline">ACK</span>/NACK bit from the controller. This
<span style="text-decoration:overline">ACK</span>/NACK bit indicates whether
the controller will keep accepting data bytes or not, i.e., the controller
sends NACK after receiving the last data byte.

Conversely, if the controller signaled a write operation, it begins sending
data bytes to the peripheral, which responds with a <span
style="text-decoration:overline">ACK</span>/NACK bit indicating the status of
the operation.

Finally, the controller sends a STOP condition to finish the data transfer.
And that's all, pretty simple so far.

### Repeated START

The bus supports a so-called repeated START condition, which means sending a
START condition following an address or data byte, i.e., without
sending a STOP condition. A new address byte comes after the repeated
START.

### 10-bit Addressing

Although rarely used, the specification foresees a 10-bit addressing mode. In
this mode, 2 address bytes are sent before data transmission can start. The
first address byte embeds a pattern in its 5 most significant bits, which is
followed by the 2 most significant address bits, and the R/<span
style="text-decoration:overline">W</span> bit: `11110XXY`. The second address
byte contains the 8 least significant address bits. Please note that the type
of operation is encoded in the first byte.

### Address Space

The specification reserves 2 sets of 7-bit addresses: `0000XXX` and `1111XXX`.
The first group is devoted entirely to special pre-defined functions; the
second group comprises 10-bit addresses and other addresses reserved for future
use.

This means that the maximum number of peripherals that can be connected to the
same I2C network is `(2^7 - 1) - 2 * (2^3 - 1) = 113`.

### Overhead

As previously stated, there's some overhead in the protocol, mainly derived
from the need to transmit an address byte to select a peripheral, and also from
the need to send again a full address byte when the operation type changes.

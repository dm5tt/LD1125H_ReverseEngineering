# What is the LD1125H?

The LD1125H is a FMCW 24GHz radar manufactured by the Chinese company Hi-Link.

It's main components consists of the 

- SGRSemi SGR1101: Analog Radar 24GHz IC for RX/TX and down-mixing
- [GigaDevice GD32F303CET6: CM4, 512k Flash, 64k RAM - not 100% compatible to its STM32 pendant](https://www.mouser.com/datasheet/2/870/GD32F303xx_Datasheet_Rev2_0-3134991.pdf)

The GD32 creates the VCO ramp using its DAC and receives the down-mixed echo signal back into a ADC channel.

# PCB
![PCB Front](./pictures/pcb_front.jpg "PCB Front")
![PCB Back](./pictures/pcb_back.jpg "PCB Back")

## Datasheets for the Radar IC

There aren't much datasheets around for the Radar IC. But it seems to be purely analog with some downmixing and VCO functionality.

These might be close to one we are looking for:

 - [SGRSemi SGR1101: Link I](http://www.szxlckj.com/mobile/products_detail.php?id=494&cid=88&search_key=&page=1)
 - [SGRSemi SGR1101: Link II](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSi1mKlVE2QAdNAQR0M2IrDAUOuMLKeAaDKWQ&s)

Also ham radio operator IK1ZYW played around with it [here](https://ik1zyw.blogspot.com/2023/09/enabling-prescaler-on-hlk-ld1115h-24.html). According to his research it's a clone of the  [Infineon BGT24LTR11N16](https://www.infineon.com/dgdl/Infineon-BGT24LTR11N16-DataSheet-v01_30-EN.pdf?fileId=5546d4625696ed7601569d2ae3a9158a).

We have the entire pin configuration and maybe the entire HF specification.

## Signals

### VCO Ramp

#### Entire Sequence
![Entire Radar Burst](./pictures/VCO_Ramp.png "Entire Radar Burst")

The entire sequence jitters heavily.

#### Bursts
![Ranging Bursts](./pictures/VCO_Ramp2.png "Ranging Bursts")

#### Bursts Detail
![Ranging Bursts Detail](./pictures/VCO_Ramp3.png "Ranging Burst Detail")

### Beat (Echo) + Ramp Signal

![Echo Signal](./pictures/Echo_Signal.png "Echo Signal")

## Firmware

Warning: I accidentally wiped the entire flash. Gonna dump the full firmware after getting a new device.


With a ST-Link adapter OpenOCD and    [this](https://github.com/gd32-rs/gd32-openocd/blob/master/target/gd32f30x.cfg) configuration file the device can be accessed.

### Connecting to the chip using OpenOCD

```# openocd -f interface/stlink.cfg  -f  ~/gd32f30x.cfg -c "init; reset halt"```


```
> flash info 0
#0 : stm32f1x at 0x08000000, size 0x00080000, buswidth 0, chipwidth 0
        #  0: 0x00000000 (0x1000 4kB) not protected
        #  1: 0x00001000 (0x1000 4kB) not protected
        #  2: 0x00002000 (0x1000 4kB) not protected
        #  3: 0x00003000 (0x1000 4kB) not protected
        #  4: 0x00004000 (0x1000 4kB) not protected
        #  5: 0x00005000 (0x1000 4kB) not protected
        #  6: 0x00006000 (0x1000 4kB) not protected
        #  7: 0x00007000 (0x1000 4kB) not protected
        #  8: 0x00008000 (0x1000 4kB) not protected
        #  9: 0x00009000 (0x1000 4kB) not protected
        # 10: 0x0000a000 (0x1000 4kB) not protected
        # 11: 0x0000b000 (0x1000 4kB) not protected
        # 12: 0x0000c000 (0x1000 4kB) not protected
        # 13: 0x0000d000 (0x1000 4kB) not protected
        # 14: 0x0000e000 (0x1000 4kB) not protected
        # 15: 0x0000f000 (0x1000 4kB) not protected
        # 16: 0x00010000 (0x1000 4kB) not protected
        # 17: 0x00011000 (0x1000 4kB) not protected
        # 18: 0x00012000 (0x1000 4kB) not protected
        # 19: 0x00013000 (0x1000 4kB) not protected
        # 20: 0x00014000 (0x1000 4kB) not protected
        # 21: 0x00015000 (0x1000 4kB) not protected
        # 22: 0x00016000 (0x1000 4kB) not protected
        # 23: 0x00017000 (0x1000 4kB) not protected
        # 24: 0x00018000 (0x1000 4kB) not protected
        # 25: 0x00019000 (0x1000 4kB) not protected
        # 26: 0x0001a000 (0x1000 4kB) not protected
        # 27: 0x0001b000 (0x1000 4kB) not protected
        # 28: 0x0001c000 (0x1000 4kB) not protected
        # 29: 0x0001d000 (0x1000 4kB) not protected
        # 30: 0x0001e000 (0x1000 4kB) not protected
        # 31: 0x0001f000 (0x61000 388kB) not protected
STM32F10x (High Density) - Rev: unknown (0x2104)
 ```

### Dumping the Flash

```
> flash read_bank 0 /tmp/output.bin
wrote 524288 bytes to file /tmp/output.bin from flash bank 0 at offset 0x00000000 in 3.247600s (157.655 KiB/s)
```


# What is the LD1125H?

The LD1125H is a FMCW 24GHz radar manufactured by the Chinese company Hi-Link.

It's main components consists of the 

- SGRSemi SGR1101: Analog Radar 24GHz IC for RX/TX and down-mixing
- [GigaDevice GD32F303CET6: Weird STM32F103 but as a Cortex M4 hybrid](https://www.mouser.com/datasheet/2/870/GD32F303xx_Datasheet_Rev2_0-3134991.pdf)

The GD32 creates the VCO ramp using its DAC and receives the down-mixed echo signal back into a ADC channel.

# PCB
![PCB Front](./pictures/pcb_front.jpg "PCB Front")
![PCB Back](./pictures/pcb_back.jpg "PCB Back")

## Datasheets for the Radar IC

There aren't much datasheets around for the Radar IC. But it seems to be purely analog with some downmixing and VCO functionality.

These might be close to one we are looking for:

 - [SGRSemi SGR1101: Link I](http://www.szxlckj.com/mobile/products_detail.php?id=494&cid=88&search_key=&page=1)
 - [SGRSemi SGR1101: Link I](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSi1mKlVE2QAdNAQR0M2IrDAUOuMLKeAaDKWQ&s)

Also ham radio operator IK1ZYW played around with it [here](https://ik1zyw.blogspot.com/2023/09/enabling-prescaler-on-hlk-ld1115h-24.html)

We have the entire pin configuration of the device but HF parameters lacking.

## Signals

### VCO Ramp

#### Entire Sequence
![Entire Radar Burst](./pictures/VCO_Ramp.png "Entire Radar Burst")

#### Bursts
![Ranging Bursts](./pictures/VCO_Ramp2.png "Ranging Bursts")

#### Bursts Detail
![Ranging Bursts Detail](./pictures/VCO_Ramp3.png "Ranging Burst Detail")

### Echo + Ramp Signal

![Echo Signal](./pictures/Echo_Signal.png "Echo Signal")

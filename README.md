# Pico-AWS-Sidewalk
AWS Sidewalk sensor framework using Raspberry Pico

[AWS Sidewalk](https://www.amazon.com/Amazon-Sidewalk/b?ie=UTF8&node=21328123011) is Amazon opening up the LoRa capabilities of its Echo devices to the public.
LoRa is a low energy, long distance radio (HAM radio frequency).  Unlike WiFi or Bluetooth which have limited range, LoRa can travel several kilometers.  In exchange it is limited in the amount of data per message (about 250bytes)

This network opens the possibility of deploying remote sensors and having them log data to a cloud system (AWS).

My project is a wave sensor, capturing wave height and period (time between wave crests) to determine wave energy - which directly relates to shoreline erosion.  This is based off of the [Smart Buoy](https://t3chflicks.org/Projects/smart-buoy/). 

## LoRa

The important thing to know about LotRa is there are two radio bands and four architectures.
### Bands
American LoRa is generally the 900 band (915mhz), while Europe standardized on the 800 band.
I am using the 915 band (which is what Sidewalk uses)
### Architecture
[Architecture](https://lora-developers.semtech.com/documentation/tech-papers-and-guides/lora-and-lorawan/) is dependent on addresses.
Device address 0 makes a peer-to-peer system where everyone talks to everyone.  Sidewalk uses this architecture, any device can talk to any other divice or gateway on the same network.
A unique device address allows for device to device specific communications.

## Hardward and Software
I am using the REYAX Rylr896 which wrappers the Semtech SX1276 Engine.  
Most boards that have the Semtech use low level socket connections and bit-banging communications.  The REYAX provides an AT serial communication interface, which makes it easy to use Python/micropython UART serial protocol programming.  You need to understand which board you are using to be sure to use the correct software library/coding style.
Coding is fairly straight-forward if you follow the [REYAX AT documentation](https://reyax.com//upload/products_download/download_file/RYLR896_EN.pdf).
The REYAX AT commands also include encryption (needed for Sidewalk).

## Setting up AWS Sidewalk
Normally AWS is accessed via HTTP commands, or MQTT (over HTTP) and talks directly to AWS.  Sidewalk uses Echo devices as gateway devices, receiving LoRa signals and passing them on to WiFi (internet).  While the AWS back-end needs to be set up to receive data, the device code is communicating LoRa to the Echo.

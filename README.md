# README

## Overview

This project is a Wi-Fi packet sniffer designed for the ESP8266 microcontroller. The program captures Wi-Fi packets in the air, parses them, and keeps track of unique MAC addresses it encounters.

## Features

- **Packet Sniffing**: Captures Wi-Fi packets using the ESP8266's promiscuous mode.
- **MAC Address Tracking**: Identifies and tracks unique MAC addresses.
- **Beacon Frame Parsing**: Parses beacon frames to extract information such as SSID and channel.
- **Client Frame Parsing**: Parses client frames to extract information such as BSSID, station address, and sequence number.

## Components

### Global Variables and Constants

- **broadcast1, broadcast2, broadcast3**: Arrays holding different types of broadcast and multicast addresses.
- **sniffing**: A boolean indicating whether sniffing is active.
- **MAXlist**: The maximum number of MAC addresses to track.
- **lastMACs**: A list of the last detected MAC addresses.
- **MACindex**: An index for the current position in the MAC address list.
- **foundMAC**: A flag indicating whether a MAC address has been found.
- **connectedMAC**: A counter for the number of unique MAC addresses detected.

### Structs

- **beaconinfo**: Holds information about beacon frames.
- **clientinfo**: Holds information about client frames.
- **RxControl**: Holds the metadata for the received packet.
- **LenSeq**: Holds the length and sequence number of the packet.
- **sniffer_buf and sniffer_buf2**: Buffers for holding the sniffer data.

### Functions

#### `parse_data`

Parses a Wi-Fi frame and extracts client information.

- **Parameters**: `frame` (the frame data), `framelen` (length of the frame), `rssi` (signal strength), `channel` (Wi-Fi channel).
- **Returns**: `clientinfo` struct with the parsed information.

#### `parse_beacon`

Parses a beacon frame and extracts information such as SSID and channel.

- **Parameters**: `frame` (the frame data), `framelen` (length of the frame), `rssi` (signal strength).
- **Returns**: `beaconinfo` struct with the parsed information.

#### `promisc_cb`

Callback function that processes captured packets and identifies unique MAC addresses.

- **Parameters**: `buf` (the buffer containing the packet data), `len` (length of the packet).
- **Functionality**:
  - Extracts the MAC address from the packet.
  - Checks if the MAC address is already in the `lastMACs` list.
  - If a new MAC address is detected, it is added to the list and printed to the serial monitor.

### ESP8266 Wi-Fi Functions

- **wifi_register_send_pkt_freedom_cb**: Registers a callback function for packet sending.
- **wifi_unregister_send_pkt_freedom_cb**: Unregisters the callback function.
- **wifi_send_pkt_freedom**: Sends a packet in freedom mode.

## Usage

1. **Setup**: Include the necessary headers and initialize the ESP8266 Wi-Fi library.
2. **Register Callback**: Use `wifi_register_send_pkt_freedom_cb` to register the `promisc_cb` function as the callback for packet capture.
3. **Start Sniffing**: The ESP8266 will start capturing packets and calling the `promisc_cb` function for each packet.
4. **Monitor Output**: Monitor the serial output to see detected MAC addresses.

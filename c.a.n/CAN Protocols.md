---
description: CAN 관련된 것
cover: ../.gitbook/assets/dd9d76e1-0c22-41ac-b8d7-1b352b3a9fc6.png
coverY: 0
---

# CAN Protocol Introduction

This section provides an in-depth overview of the purpose, functionality, and applications of the CAN (Controller Area Network) protocol.

## Purpose and Functionality of CAN Protocol

The CAN protocol, developed initially for automotive use, is a message-based protocol designed to allow microcontrollers and devices to communicate without a host computer. It is essential in multiplex electrical wiring within vehicles and is increasingly used in other industrial contexts.

### Historical Development

- **Origin**: Developed in 1983 at Robert Bosch GmbH and released in 1986.
- **Evolution**: First adopted in the Mercedes-Benz W140 in 1991, with continuous advancements like CAN 2.0 and CAN FD.

## Areas of Application

CAN protocol's applications extend beyond its initial automotive purpose:

- **Automotive Systems**: Links various electronic control units (ECUs) within vehicles.
- **Industrial Automation**: Facilitates communication in complex industrial systems.
- **Other Applications**: Used in medical instruments, building automation, agricultural machinery, and more.

# CAN Message Frames

A critical aspect of CAN protocol is its message frame structure which ensures efficient and prioritized communication.

## Standard vs. Extended CAN Frames

- **Standard CAN Frames**: Utilize an 11-bit identifier suitable for most applications.
- **Extended CAN Frames**: Employ a 29-bit identifier for more complex systems, allowing more data payloads and greater flexibility.

## Frame Structure

A typical CAN frame includes:

- **Identifier**: Determines the message's priority and type.
- **Control Field**: Indicates the size of the data field.
- **Data Field**: Contains the payload of up to 8 bytes.
- **CRC**: Ensures data integrity.
- **ACK**: Confirms message receipt.

# Message Transmission and Reception

Understanding message transmission and reception processes is crucial in CAN networks.

## Transmission Process

- Details on message broadcasting, arbitration, and collision handling.
- **Arbitration**: Explains how the CAN protocol handles simultaneous message transmission using identifier-based priority.

## Error Detection and Correction Mechanisms

- Highlights the role of CRC and ACK in maintaining reliable communication.
- **Error Handling**: Discusses how the CAN protocol detects and corrects errors.

## Message Priority

- Delve into how the identifier field influences message transmission priority in the network.

# CAN Network Topology

CAN networks can be configured in various topologies based on application requirements.

## Physical Configuration

- **Bus Topology**: The most common configuration in vehicles.
- **Star and Ring Topologies**: Used in more complex or larger-scale applications.

# Error Management and Network Security

In-depth analysis of error handling and security considerations in CAN networks.

## Types of Errors in CAN Networks

- Categorization and impact of different error types.
- **Common Errors**: Bit errors, frame errors, and error propagation.

## Error Detection and Recovery Mechanism

- Mechanisms like error counters and automatic retransmission in error handling.
- **Fault Confinement**: How CAN systems prevent faulty nodes from disrupting the network.

## Considerations for Network Security

- Challenges in securing CAN networks against external threats and potential solutions.

# Protocol Extensions and Current Trends

A look at the latest developments and future directions of CAN technology.

## CAN FD (Flexible Data-rate)

- Introduction to CAN with Flexible Data-Rate (CAN FD), offering higher data rates and larger payload capacity.
- **Advantages of CAN FD**: Improved bandwidth and efficiency over traditional CAN.

## New Applications and Directions

- Exploring emerging use cases of CAN in various industries, including advancements in autonomous vehicles and smart industrial systems.

## Ongoing Development

- Discussion on the continual development and adaptation of the CAN protocol to meet evolving technological needs.

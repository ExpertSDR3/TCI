# Transceiver Control Interface

## Introduction

TCI (Transceiver Control Interface) is a network interface for control, data transfer and
synchronization between transceiver/receiver, contest loggers, digital mode software, skimmers
and other software, as well as external power amplifiers, bandpass filter units, antenna switches,
radio controllers and other devices.

TCI was created as a modern alternative to the outdated COM port and audio cable
interfaces, it uses a full duplex web socket protocol that runs on top of a TCP connection and
serves for server-client communications, providing cross-platform connectivity. Transceiver works
as a server, all other software and devices as clients. The server and clients can be inside the
same computer (program-server, hardware log, etc.-clients) and/or in separate physical devices
connected through the local network (classical transceiver, power amplifier, antenna switch, FFT
unit, etc.).

The TCI interface contains basic transceiver control commands (analog of CAT system),
receives CW macros from clients and broadcasts them, outputs transceiver IQ stream to clients,
receives spots from skimmers and Internet clusters, receives/outputs audio signal to work in
digital modes.

The TCI uses an extensible architecture and can be supplemented with new functions and
commands, while keeping the old ones operational. Thus, the TCI interface can be extended and
supplemented to meet the specific needs of any software manufacturer and/or device
manufacturer (receivers, transceivers, power amplifiers, switches, etc.). The presence of a device
identifier allows the manufacturers of transceivers and receivers to switch to the TCI interface
while maintaining the device model designation. The extensibility of the TCI interface allows you
to create an individual set of commands and functions for each device model, while maintaining
the basic command set inherent to all transceivers.

Our company advocates universal unification of data exchange between devices and
software by creating the TCI interface for this purpose. Modern transceivers and software must
communicate using one protocol - the TCI protocol.

## Interface description

Any command represents an ASCII string that contains a command name and a list of
arguments corresponding to this command. There are reserved characters that cannot be
included in the command name and command arguments.

List of reserved characters: «:», «,», «;».

Command structure:
1. Name of the command;
2. Separating character between command name and arguments «:»;
3. Separating character between arguments «,»;
4. End of the command character «;».

If a command has no arguments, an end of command symbol is placed after the command
name. If the command is invalid, it is ignored. The case of letters does not matter.

The ExpertSDR3 program acts as a server, which can have several client connections at
the same time, they will be synchronized with each other by the server. When connecting to the
ExpertSDR3, the client receives the current status of the ExpertSDR3, first sending initialization
commands, then parameters to set the status, such as frequency, modulation, etc.

When a parameter change occurs in the ExpertSDR3 (server) program, the server notifies
all connected clients, i.e., clients do not need to poll the server constantly, any change of state
will be sent in time to all clients. If the client sends a new state, the server will set it to itself, as
well as send it to all clients, that is, the server acts as a synchronizer. All clients connected to the
server will be automatically synchronized. This way of work allows to minimize network load,
reducing traffic.

The TCI protocol implements the transmission of receiver IQ stream to clients, which is
necessary for the work of special skimmer software, they automatically find the station and
decode it throughout the band, and it also allows you to record radio signals in the file.

TCI is also used to transmit audio signals of the receiver to clients and to receive audio
signals from clients, i.e., the client can transmit audio signals to ExpertSDR3 for radio
transmission. The audio stream exchange is designed to work with digital modes, where encoding
and decoding is performed by third-party software, as well as in voice modes, where audio macros
can be broadcasted, which is very much in demand in contest loggers.

When working in contests, it is important to record all on the air operation, for this purpose
the audio stream from the line-output is sent to all clients. The resulting audio stream can be
recorded to a file or played back with a PC sound card.

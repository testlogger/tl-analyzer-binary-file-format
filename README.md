Introduction

============

This document describes a binary file format for TestLogger Analyzer.

File format is open source and free for everyone to utilise as they wish

Basic Types

===========

The following basic types are used as terminals in the rest of the grammar. Each type must be serialized in little-endian format.

| Type | Description |
| --- | --- |
| byte | 1 byte (8-bits) |
| int16 | 2 bytes (16-bit signed integer, two's complement) |
| uint16 | 2 bytes (16-bit unsigned integer) |
| int32 | 4 bytes (32-bit signed integer, two's complement) |
| uint32 | 4 bytes (32-bit unsigned integer) |
| int64 | 8 bytes (64-bit signed integer, two's complement) |
| uint64 | 8 bytes (64-bit unsigned integer) |
| double | 8 bytes (64-bit IEEE 754-2008 binary floating point) |
| decimal128 | 16 bytes (128-bit IEEE 754-2008 decimal floating point) |

Spec
====

Header
------
Header defines the magic number and locations where different file sections start
| Data | Location | Length |
| --- | --- | --- |
| Magic number | 0 | 4 |
| File format version | 4 | 4 |
| Meta data start | 8 | 4 |
| Config start | 12 | 4 |
| Data start | 16 | 4 |
| Laptime channel ID | 20 | 4 |

Run meta data
-------------
| Param | Type | Size |
| --- | --- | --- |
| Logging device | String | 64 |
| Serial number | Uint32 | 4 |
| Date and time in unix time | Int32 | 4 |
| Environment UUID | String | 36 |
| Session name | String | 128 |
| Session ID | Uint32 | 4 |
| Session UUID | String | 36 |
| Driver name | String | 128 |
| Driver ID | Uint32 | 4 |
| Driver UUID | String | 36 |
| Car name | String | 128 |
| Car ID | Uint32 | 4 |
| Car UUID | String | 36 |
| Track name | String | 128 |
| Track ID | Uint32 | 4 |
| Track UUID | String | 36 |
| Run name | String | 128 |
| Run ID | Uint32 | 4 |
| Run UUID | String | 36 |
| Setup Name | String | 128 |
| Setup ID | Uint32 | 4 |
| Setup UUID | String | 36 |
| Comments short | String | 256 |
| Comments long | String | 2048 |
| Environment UUID | String | 36 |

Channel config
--------------
| Param | Type | Size | Value / options |
| --- | --- | --- | --- |
| Channel definition start | Uint16 | 2 | 20111 |
| Channel ID | Uint16 | 2 | |
| Sample rate | Uint16 | 2 | 1, 10, 100, 250, 500 |
| Sample count | Uint32 | 4 | |
| Sample start | Uint32 | 4 | |
| Value type | Uint16 | 2 | |
| Value size | Uint16 | 2 | |
| Decimals | Uint16 | 2 | |
| Offset | Uint16 | 2 | |
| Gain | Uint16 | 2 | |
| Channel name | String | 64 | |
| Channel unit | String | 8 | |
| Reserved | String | 256 | |
| Channel definition end | Uint16 | 2 | 20222 |

Data
----
Measured samples as continuous stream.

Special channels
----------------
Laptriggers needs to be handled with a custom channel. This channel shall be recorded in 10Hz

| Data | Location | Length | Value  |
| --- | --- | --- | --- |
| Magic number | 0 | 1 | -120 |
| Laptrig type | 1 | 1 | -10 for lap, -15 for split |
| Counter | 2 | 2 | uint16 |
| Time from start in milliseconds | 4 | 4 | Int32 |

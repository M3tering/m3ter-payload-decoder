# M3ter Payload Decoder

A web-based decoder for M3tering protocol payload data. This tool allows you to decode binary payloads from smart meters into human-readable format, supporting both hex and base64 input formats.

## Decoded Fields

### Core Fields (Always Present)
- **Nonce**: 32-bit counter for replay attack prevention
- **Energy**: Cumulative energy reading in kWh (6 decimal precision)
- **Signature**: 64-byte Ed25519 digital signature

### Extension Fields (Optional)
- **Voltage**: Current voltage reading in volts (1 decimal precision)
- **Device ID**: 32-byte unique device identifier
- **Longitude**: Geographic longitude in degrees (5 decimal precision)
- **Latitude**: Geographic latitude in degrees (5 decimal precision)

### Example Payloads

#### Hex Format
```
0000000000001aea1b0bb5a800ab9647c6bdaae5b915d1a1cfa406cc2129fb94d5841d73fca553329e1fe9c7cf52df89ff9086ba5929825739f72b55f1538448e22baaedf454db050082356963ce530d3dc24cbb2c0870923171442efbb202de75b0572da97b22de9767
```

#### Base64 Format
```
AAAAAAAAGuobC7WoAKuWR8a9quW5FdGhz6QGzCEp+5TVhB1z/KVTMp4f6cfPUt+J/5CGulkpglc59ytV8VOESOIrqu30VNsFAII1aWPOUw09wky7LAhwkjFxRC77sgLedbBXLal7It6XZw==
```

## Protocol Specification

The M3ter payload follows a specific binary format:

```
Offset  Length  Field        Description
------  ------  -----------  ----------------------------------
0       4       Nonce        Big-endian uint32 counter
4       4       Energy       Big-endian uint32 (kWh × 10⁶)
8       64      Signature    Ed25519 signature over bytes 0-7
[72]    2       Voltage      Big-endian uint16 (V × 10)
[74]    32      Device ID    32-byte identifier
[106]   3       Longitude    Signed 24-bit int (degrees × 10⁵)
[109]   3       Latitude     Signed 24-bit int (degrees × 10⁵)
```

## Technical Details

### Data Encoding
- **Integers**: Big-endian byte order
- **Energy**: Multiplied by 10⁶ for 6 decimal places precision
- **Voltage**: Multiplied by 10 for 1 decimal place precision
- **Coordinates**: Multiplied by 10⁵ for 5 decimal places precision
- **Signature**: Ed25519 algorithm over nonce + energy bytes

**Note**: This decoder is for development and testing purposes. Always verify decoded data against your specific implementation requirements.

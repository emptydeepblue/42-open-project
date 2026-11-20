# Emergency Mesh Network

A messaging network that works **without internet or a cellular network**. ESP32 devices with LoRa radios form a mesh: every node relays the others' messages, so a message travels far by hopping from device to device. The protocol is written from scratch.

Open Project — 42 Firenze.

## Status

📝 Phase 1: writing the specification, awaiting validation.

## Documentation

- [Project specification](docs/specification.md)

## Structure

```
docs/        specification and documentation
```

## Contributing

1. Create a branch from `main`
2. Open a pull request: it needs approval from at least one other member
3. Open decisions are discussed in issues, one per topic

## Team

| Member | Role |
| --- | --- |
| Emma | Lead, protocol and hardware |
| — | Device firmware |
| — | Security |
| — | Interface |
| — | Testing and integration |

## License

To be decided together.
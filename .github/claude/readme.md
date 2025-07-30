# py-libp2p-daemon-bindings

Python bindings for the libp2p daemon, providing a comprehensive client library to interact with official libp2p daemons. This library enables peer-to-peer networking capabilities in Python applications through protobuf communication with Go and JavaScript libp2p daemon implementations.

## 🚀 Features

- **Cross-Daemon Compatibility**: Works with both Go and JavaScript libp2p daemons
- **Comprehensive P2P Operations**: Support for peer identification, connection management, and stream handling
- **DHT Operations**: Distributed Hash Table functionality for peer discovery and content routing
- **PubSub Messaging**: Publisher-subscriber pattern implementation for decentralized messaging
- **Connection Management**: Advanced connection lifecycle and resource management
- **Stream Handling**: Bidirectional stream creation and management for custom protocols
- **Type Safety**: Full mypy type checking support with stub files included
- **Testing Utilities**: Built-in daemon spawning capabilities for testing and development

## 🛠️ Tech Stack

### Core Technologies
- **Python 3.7+**: Primary programming language with async/await support
- **Protocol Buffers**: Message serialization for daemon communication
- **libp2p**: Modular peer-to-peer networking framework

### Development Tools
- **pytest**: Testing framework with comprehensive test coverage
- **mypy**: Static type checking for enhanced code reliability
- **black**: Code formatting and style consistency
- **tox**: Multi-environment testing automation
- **setuptools**: Package building and distribution

### Networking & Communication
- **gRPC/Protobuf**: High-performance RPC communication with daemons
- **Multiaddr**: Universal addressing format for network endpoints
- **Crypto Libraries**: RSA and other cryptographic operations for peer identity

## 📁 Project Structure

```
p2pclient/
├── __init__.py          # Package initialization and public API
├── daemon.py            # Daemon spawning and management
├── control.py           # Core control protocol implementation
├── p2pclient.py         # Main client class and connection handling
├── pubsub.py            # Publisher-subscriber messaging
├── dht.py               # Distributed Hash Table operations
├── connmgr.py           # Connection manager functionality
├── datastructures.py   # Core data structures and models
├── serialization.py     # Message serialization utilities
├── utils.py             # Utility functions and helpers
├── exceptions.py        # Custom exception definitions
├── config.py            # Configuration management
├── libp2p_stubs/        # Type stubs from py-libp2p
│   ├── crypto/          # Cryptographic key management
│   └── peer/            # Peer identity and information
└── pb/                  # Protocol buffer definitions
    ├── p2pd.proto       # Daemon protocol specification
    └── p2pd_pb2.py      # Generated Python protobuf code
```

## 🔧 Installation & Setup

### Prerequisites
- Python 3.7 or higher
- Go 1.12+ (for Go daemon)
- Node.js 16+ (for JavaScript daemon)
- libp2p daemon binary

### Installation Steps

1. **Install the Python package**:
```bash
pip install py-libp2p-daemon-bindings
```

2. **Install libp2p daemon** (choose one):
```bash
# Option 1: Use the provided script
./scripts/install_p2pd.sh

# Option 2: Install Go daemon manually
go get -d github.com/libp2p/go-libp2p-daemon
cd $GOPATH/src/github.com/libp2p/go-libp2p-daemon
make install

# Option 3: Install JavaScript daemon
npm install -g libp2p-daemon
```

3. **Development installation**:
```bash
git clone https://github.com/anisharma07/py-libp2p-daemon-bindings.git
cd py-libp2p-daemon-bindings
pip install -e .
```

## 🎯 Usage

### Basic Client Usage

```python
import asyncio
from p2pclient import Client, Daemon

async def main():
    # Start a daemon instance
    daemon = Daemon()
    await daemon.start()
    
    # Connect to the daemon
    client = Client(daemon.control_maddr)
    await client.listen("/ip4/127.0.0.1/tcp/0")
    
    # Get peer information
    peer_id = await client.identify()
    print(f"Peer ID: {peer_id}")
    
    # Connect to another peer
    await client.connect(peer_id, [multiaddr])
    
    # Clean up
    await client.close()
    await daemon.close()

asyncio.run(main())
```

### PubSub Messaging

```python
from p2pclient import Client

async def pubsub_example():
    client = Client()
    
    # Subscribe to a topic
    async def message_handler(msg):
        print(f"Received: {msg.data}")
    
    await client.pubsub_subscribe("my-topic", message_handler)
    
    # Publish a message
    await client.pubsub_publish("my-topic", b"Hello, libp2p!")
```

### DHT Operations

```python
async def dht_example():
    client = Client()
    
    # Put value in DHT
    await client.dht_put(b"key", b"value")
    
    # Get value from DHT
    value = await client.dht_get(b"key")
    print(f"DHT value: {value}")
    
    # Find peers
    peers = await client.dht_find_peers("topic")
```

### Development with Daemon Spawning

```python
from p2pclient import Daemon

# For testing - spawn daemon programmatically
daemon = Daemon(
    is_pubsub_enabled=True,
    is_dht_enabled=True,
    is_connmgr_enabled=True
)
await daemon.start()

# Use daemon.control_maddr to connect clients
client = Client(daemon.control_maddr)
```

## 📊 Daemon Compatibility

### Go Daemon (v0.2.0) - Full Support
- ✅ Identify
- ✅ Connect
- ✅ StreamOpen
- ✅ StreamHandler (Register & Inbound)
- ✅ DHT operations
- ✅ Connection manager operations  
- ✅ PubSub operations

### JavaScript Daemon (v0.10.2) - Partial Support
- ✅ Identify
- ✅ Connect  
- ✅ StreamOpen
- ✅ StreamHandler (Register & Inbound)
- ✅ PubSub operations
- ❌ DHT operations (buggy/incomplete)
- ❌ Connection manager operations
- ❌ PeerStore operations

## 🧪 Testing

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=p2pclient

# Run specific test categories
pytest tests/test_p2pclient.py
pytest tests/test_p2pclient_integration.py

# Run tests across multiple Python versions
tox
```

### Test Configuration

The project includes comprehensive test coverage:
- Unit tests for core functionality
- Integration tests with live daemons
- Interoperability tests with libp2p components
- Type checking validation tests

## 🔄 Development

### Code Quality Tools

```bash
# Format code with black
black p2pclient/

# Type checking with mypy
mypy p2pclient/

# Run linting
flake8 p2pclient/
```

### Building from Source

```bash
# Build wheel and source distribution
python -m build --sdist --wheel --outdir dist/

# Install development dependencies
pip install -e ".[dev]"
```

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 style guidelines (enforced by black)
- Add type hints for all public APIs
- Include comprehensive tests for new features
- Update documentation for API changes
- Ensure compatibility with supported Python versions (3.7-3.9)
- Run the full test suite before submitting PRs

### Code Style

This project uses:
- **black** for code formatting
- **mypy** for type checking
- **pytest** for testing
- **flake8** for additional linting

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

The libp2p stub files are dual-licensed under Apache 2.0 and MIT licenses from the original [py-libp2p](https://github.com/libp2p/py-libp2p) project.

## 🙏 Acknowledgments

- **mhchia** - Original creator and maintainer
- **libp2p community** - For the excellent peer-to-peer networking framework
- **Protocol Labs** - For advancing decentralized web technologies
- **py-libp2p project** - For foundational cryptography and peer identity code

## 📞 Support & Contact

- **Issues**: [GitHub Issues](https://github.com/anisharma07/py-libp2p-daemon-bindings/issues)
- **Discussions**: [GitHub Discussions](https://github.com/anisharma07/py-libp2p-daemon-bindings/discussions)
- **libp2p Documentation**: [docs.libp2p.io](https://docs.libp2p.io/)
- **Protocol Specification**: [libp2p daemon spec](https://github.com/libp2p/specs/tree/master/daemon)

For urgent issues or security concerns, please create a GitHub issue with appropriate labels.

---

[![Unit Tests](https://github.com/anisharma07/py-libp2p-daemon-bindings/actions/workflows/unit-tests.yml/badge.svg)](https://github.com/anisharma07/py-libp2p-daemon-bindings/actions/workflows/unit-tests.yml)
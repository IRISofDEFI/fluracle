# fluracle
# Fluracle

**Smart Price Oracle for Fluent Network**

Next-generation Web3 price oracle powered by Rust mathematics and Solidity compatibility. Real-time crypto prices with advanced volatility analysis.

## Overview

Fluracle demonstrates Fluent's blended execution capabilities by combining:
- **Rust Processing**: Complex mathematical calculations for moving averages and volatility analysis
- **Solidity Compatible**: Standard oracle interface that existing DeFi protocols expect
- **Real-time Analytics**: Beyond raw prices - provides processed market insights

## Features

- **Smart Price Data**: Current prices with 24-hour moving averages
- **Volatility Analysis**: Real-time volatility scoring in basis points
- **Multi-Asset Support**: ETH, BTC, and extensible for additional cryptocurrencies
- **Gas Efficient**: Heavy calculations done in Rust, results stored efficiently
- **Standard Interface**: Compatible with existing oracle integrations

## Architecture

### Blended Execution Design

Fluracle showcases Fluent's unique blended execution by using:

**Rust Components** (Performance-Critical):
- Moving average calculations
- Volatility analysis algorithms  
- Price history management
- Mathematical operations

**Solidity Interface** (Ecosystem Compatibility):
- Standard oracle functions (`getPrice`, `getMovingAverage`)
- Event emissions for transparency
- Storage mappings for data persistence
- EVM-compatible ABI

## Quick Start

### Prerequisites

- Docker (for reproducible builds)
- gblend CLI tool
- EVM-compatible wallet with testnet ETH

### Installation

1. **Install gblend**
```bash
curl -sSL https://raw.githubusercontent.com/fluentlabs-xyz/gblend/refs/tags/latest/gblendup/install | bash
source ~/.bashrc
gblendup
```

2. **Clone and Setup**
```bash
git clone <your-repo-url>
cd fluracle
```

3. **Build Contract**
```bash
gblend build
```

4. **Deploy to Testnet**
```bash
# Add your private key to .env file first
echo "PRIVATE_KEY=your_key_here" > .env
source .env

gblend create Fluracle.wasm \
--rpc-url https://rpc.testnet.fluent.xyz \
--private-key $PRIVATE_KEY \
--broadcast \
--verify \
--wasm \
--verifier blockscout \
--verifier-url https://testnet.fluentscan.xyz/api/
```

## Usage

### For Developers

**Basic Price Query:**
```solidity
// Import auto-generated interface
import "../out/Fluracle.wasm/interface.sol";

contract MyDeFiApp {
    IFluracle oracle = IFluracle(FLURACLE_ADDRESS);
    
    function getETHPrice() public view returns (uint256) {
        return oracle.getPrice(1); // 1 = ETH
    }
    
    function getVolatilityScore() public view returns (uint256) {
        return oracle.getVolatility(1); // Returns basis points
    }
}
```

**Advanced Analytics:**
```solidity
function isMarketStable() public view returns (bool) {
    uint256 volatility = oracle.getVolatility(1);
    return volatility < 500; // Less than 5% volatility
}

function getPriceWithTrend() public view returns (uint256, uint256, bool) {
    uint256 price = oracle.getPrice(1);
    uint256 average = oracle.getMovingAverage(1);
    bool trending_up = price > average;
    return (price, average, trending_up);
}
```

### Asset IDs

- `1` = Ethereum (ETH)
- `2` = Bitcoin (BTC)

### Functions

| Function | Description | Returns |
|----------|-------------|---------|
| `getPrice(uint256 assetId)` | Current asset price | Price in wei (18 decimals) |
| `getMovingAverage(uint256 assetId)` | 4-period moving average | Average price in wei |
| `getVolatility(uint256 assetId)` | Volatility score | Basis points (100 = 1%) |
| `updatePrice(uint256 assetId, uint256 price)` | Update asset price | Success indicator |
| `getLastUpdated(uint256 assetId)` | Last update timestamp | Unix timestamp |
| `getAssetName(uint256 assetId)` | Asset symbol | String ("ETH", "BTC") |

## Technical Specifications

### Computation Engine
- **Language**: Rust with Fluentbase SDK
- **Mathematics**: Moving averages, volatility calculations
- **Performance**: Sub-millisecond calculations
- **Optimization**: Gas-efficient storage patterns

### Integration
- **Interface**: Solidity-compatible ABI
- **Standards**: Standard oracle functions
- **Events**: Price updates and volatility changes
- **Compatibility**: Works with existing DeFi protocols

### Data Insights
- **Trend Analysis**: Moving average comparisons
- **Risk Assessment**: Volatility scoring
- **Historical Context**: Price history tracking
- **Smart Analytics**: Processed data, not raw feeds

## Development

### Project Structure
```
fluracle/
├── src/
│   └── fluracle/
│       ├── Cargo.toml
│       └── src/
│           └── lib.rs          # Oracle contract
├── out/                        # Build artifacts
│   └── Fluracle.wasm/
│       └── interface.sol       # Auto-generated Solidity interface
└── README.md
```

### Building from Source
```bash
gblend build
```

### Testing
```bash
gblend test
```

### Local Development
For local testing, the contract initializes with sample data:
- ETH: $2,000 with 18 decimals
- BTC: $45,000 with 18 decimals

## Network Information

### Fluent Testnet
- **RPC URL**: https://rpc.testnet.fluent.xyz
- **Chain ID**: 20994
- **Explorer**: https://testnet.fluentscan.xyz
- **Faucet**: https://testnet.gblend.xyz

## Why Fluracle?

Traditional oracles provide raw price feeds. Fluracle provides smart analytics:

**Traditional Oracle**: "ETH costs $2,000"  
**Fluracle**: "ETH costs $2,000, trending up from $1,980 average, volatility at 2.5%"

This processed data helps developers make smarter contract decisions without expensive on-chain calculations.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## Roadmap

- [ ] Additional asset support (SOL, AVAX, etc.)
- [ ] External price feed integration
- [ ] Advanced statistical models
- [ ] Multi-timeframe moving averages
- [ ] Yield farming integration examples

## License

MIT License - see LICENSE file for details

## Support

- **Discord**: Join the Fluent builder community
- **Documentation**: Check Fluent's official docs
- **Issues**: Report bugs via GitHub issues

## Acknowledgments

Built during the early days of Fluent's blended execution network. Special thanks to the Fluent Labs team for their innovative architecture that makes this kind of cross-VM composability possible.

---

*Fluracle demonstrates the future of Web3 development: choosing ALL sides instead of picking one.*

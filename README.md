# Fractal BRC-20 Token Swap Service

This service enables the swapping of BRC-20 tokens, based on the BRC-20 token standard of ordinal theory, between two different types of tokens by creating a liquidity pool.  

This project implements a decentralized Automated Market Maker (AMM) to facilitate the swapping of BRC-20 tokens conforming to the ordinal theory. It allows for swaps between BRC-20 tokens and other token types by utilizing a liquidity pool mechanism.

## Introduction

The Fractal BRC-20 Token Swap Service is designed to provide a seamless mechanism for swapping BRC-20 tokens. It is built around the concept of liquidity pools, which enables efficient token swaps without the need for traditional order books.

## Features

- Support for BRC-20 token swaps.
- Integration with CAT20 tokens.
- Automatic price discovery and swap execution.
- Liquidity pool creation and management.
- Smart contract-based automation for secure and trustless swaps.

## Architecture Overview

The service comprises several key components:

- **Liquidity Pools**: Central to the AMM, allowing users to deposit tokens and earn fees.
- **Swap Interface**: Facilitates user interaction, including token swaps and pool management.
- **Backend Services**: To handle off-chain calculations and optimizations.

## Installation

1. **Prerequisites**: 
   - Node.js (latest LTS version recommended)
   - npm or yarn
   - A supported blockchain client (e.g., Ethereum, BRC-20 compatible chain)

2. **Clone the Repository**:
   ```bash
   git clone https://github.com/rezzecup/fractal-brc20-token-swap-service.git
   cd fractal-brc20-token-swap-service
   ```

3. **Install Dependencies**:
   ```bash
   npm install
   ```

4. **Configuration**:
   - Configure environment variables in `.env` file. (e.g., API keys, blockchain node endpoints)

5. **Run the Application**:
   ```bash
   npm start
   ```

## Usage

1. **Access the Swap Interface**: Visit `http://localhost:3000` (default port) to access the user-friendly swap interface.

2. **Swapping Tokens**:
   - Select the token pair to swap.
   - Enter the amount and confirm the transaction.
   - Approve the swap through your connected wallet.

3. **Providing Liquidity**:
   - Navigate to the 'Liquidity' section.
   - Deposit tokens into a pool to earn a share of the swap fees.

## AMM Implementation

The AMM mechanism follows the constant product formula \( x \times y = k \), where `x` and `y` are the reserves of the two tokens in a pool, and `k` is a constant. This approach automatically adjusts prices based on supply and demand.

### Key Functions:

- **addLiquidity**: Adds tokens to a pool.
- **removeLiquidity**: Withdraws a share of tokens from a pool.
- **swapTokens**: Executes a swap between two tokens according to the AMM model.

Creating the core functionality of an Automated Market Maker (AMM) with BRC-20 support involves implementing smart contract interactions, liquidity pool mechanics, and swap functionality. Below is a simplified example that provides a starting point.

### Directory Structure

The code structure is organized into a TypeScript project:

```
fractal-brc20-token-swap-service/
├── src/
│   ├── models/
│   │   ├── Token.ts
│   │   ├── LiquidityPool.ts
│   ├── services/
│   │   ├── AMMService.ts
│   ├── utils/
│   │   ├── BlockchainConnector.ts
│   │   ├── MathUtils.ts
│   ├── index.ts
└── tsconfig.json
```

### src/models/Token.ts

```typescript
export class Token {
  constructor(public address: string, public symbol: string, public decimals: number) {}
}
```

### src/models/LiquidityPool.ts

```typescript
import { Token } from './Token';

export class LiquidityPool {
  constructor(public tokenA: Token, public tokenB: Token, public reserveA: number, public reserveB: number) {}

  // Mocked method to update reserves
  updateReserves(newReserveA: number, newReserveB: number) {
    this.reserveA = newReserveA;
    this.reserveB = newReserveB;
  }
}
```

### src/services/AMMService.ts

```typescript
import { LiquidityPool } from '../models/LiquidityPool';
import { MathUtils } from '../utils/MathUtils';

export class AMMService {
  constructor(private pool: LiquidityPool) {}

  // Simple swap logic
  swap(tokenIn: string, amountIn: number): number {
    if (tokenIn === this.pool.tokenA.address) {
      const amountOut = MathUtils.getAmountOut(amountIn, this.pool.reserveA, this.pool.reserveB);
      this.pool.updateReserves(this.pool.reserveA + amountIn, this.pool.reserveB - amountOut);
      return amountOut;
    } else if (tokenIn === this.pool.tokenB.address) {
      const amountOut = MathUtils.getAmountOut(amountIn, this.pool.reserveB, this.pool.reserveA);
      this.pool.updateReserves(this.pool.reserveA - amountOut, this.pool.reserveB + amountIn);
      return amountOut;
    } else {
      throw new Error('Invalid token address');
    }
  }

  // Liquidity management functions
  addLiquidity(amountA: number, amountB: number) {
    this.pool.updateReserves(this.pool.reserveA + amountA, this.pool.reserveB + amountB);
  }

  removeLiquidity(amountA: number, amountB: number) {
    this.pool.updateReserves(this.pool.reserveA - amountA, this.pool.reserveB - amountB);
  }
}
```

### src/utils/MathUtils.ts

```typescript
export class MathUtils {
  // Computes the amount out for a given amount in
  static getAmountOut(amountIn: number, reserveIn: number, reserveOut: number): number {
    const amountInWithFee = amountIn * 0.997; // Assuming a 0.3% fee
    const numerator = amountInWithFee * reserveOut;
    const denominator = reserveIn + amountInWithFee;
    return numerator / denominator;
  }
}
```

### src/utils/BlockchainConnector.ts

```typescript
// Placeholder for blockchain interaction logic
export class BlockchainConnector {
  // Example for initializing Ethers.js, make sure to adjust for your use case
  // ...

  constructor(private providerUrl: string) {}

  // Mocked method to retrieve token details
  async getTokenDetails(tokenAddress: string) {
    // Replace with actual blockchain call logic
    return { address: tokenAddress, symbol: 'MOCK', decimals: 18 };
  }
}
```

### src/index.ts

```typescript
import { Token } from './models/Token';
import { LiquidityPool } from './models/LiquidityPool';
import { AMMService } from './services/AMMService';

// Initialize tokens
const tokenA = new Token('0xTokenA', 'TKNA', 18);
const tokenB = new Token('0xTokenB', 'TKNB', 18);

// Create a liquidity pool
const pool = new LiquidityPool(tokenA, tokenB, 100000, 100000);

// Initialize AMM service with the pool
const ammService = new AMMService(pool);

// Simulate a swap
const outputAmount = ammService.swap(tokenA.address, 100);
```


## CAT20 Token Support

The service also extends functionality to the CAT20 token standard. This involves additional smart contract logic and interface adjustments to ensure compatibility and functionality.

## Contact Info:

If you have technical issues & development inquires, please contact here.

- Twitter: https://x.com/rez_cats/
- Telegram: https://t.me/rizz_cat/


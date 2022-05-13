# Timeless

Timeless is a yield tokenization protocol that offers _Perpetual Yield Tokens_ (PYTs), which give their holders a perpetual right to claim the yield generated by the underlying principal. Timeless also offers _Negative Yield Tokens_ (NYTs), a protocol-native way to short yield rates.

1 perpetual yield token represents the right to claim the yield generated by 1 underlying token from now to forever in the future.

## Overview

1. Deposit `x` underlying tokens (e.g. DAI), receive `x` _perpetual yield tokens_ and `x` _negative yield tokens_.
2. The DAI is deposited into yield protocols like Yearn.
3. A holder of `x` PYTs can claim the yield earned by `x` DAI since the last claim at any time.
4. In order to redeem `x` DAI from the protocol, one must burn `x` PYT and `x` NYT together.

## Architecture

-   [`Gate.sol`](src/Gate.sol): Abstract contract that mints/burns NYTs and PYTs of vaults of a specific protocol. Allows PYT holders to claim the yield earned by PYTs. Owns all vault shares.
-   [`Factory.sol`](src/Factory.sol): Deploys NYT and PYT contracts. Tracks protocol fee info.
-   [`NegativeYieldToken.sol`](src/NegativeYieldToken.sol): ERC20 token for representing NYTs.
-   [`PerpetualYieldToken.sol`](src/PerpetualYieldToken.sol): ERC20 token for representing PYTs.
-   [`external/`](src/external/): Interfaces for external contracts Timeless interacts with.
    -   [`IxPYT.sol`](src/external/IxPYT.sol): Interface for xPYT vaults.
    -   [`YearnVault.sol`](src/external/YearnVault.sol): Interface for Yearn v2 vaults.
-   [`gates/`](src/gates/): Implementations of `Gate` integrated with different yield protocols.
    -   [`ERC20Gate.sol`](src/gates/ERC20Gate.sol): Abstract implementation of `Gate` for protocols using ERC20 vault shares.
    -   [`YearnGate.sol`](src/gates/YearnGate.sol): Implementation of `Gate` that uses Yearn v2 vaults.
    -   [`ERC4626Gate.sol`](src/gates/ERC4626Gate.sol): Implementation of `Gate` that uses ERC4626 vaults.
-   [`lib/`](src/lib/): Libraries used by other contracts.
    -   [`BaseERC20.sol`](src/lib/BaseERC20.sol): The basic gate-controlled ERC20 class used by `PerpetualYieldToken` and `NegativeYieldToken`.
    -   [`ERC20.sol`](src/lib/ERC20.sol): The ERC20 implementation used by `BaseERC20`.
    -   [`FullMath.sol`](src/lib/FullMath.sol): Math library preventing phantom overflows during mulDiv operations.
    -   [`Multicall.sol`](src/lib/Multicall.sol): Enables calling multiple methods in a single call to the contract.
    -   [`SelfPermit.sol`](src/lib/SelfPermit.sol): Functionality to call permit on any EIP-2612-compliant token.

## Installation

To install with [DappTools](https://github.com/dapphub/dapptools):

```
dapp install timeless-fi/timeless
```

To install with [Foundry](https://github.com/gakonst/foundry):

```
forge install timeless-fi/timeless
```

## Local development

This project uses [Foundry](https://github.com/gakonst/foundry) as the development framework.

### Dependencies

```
make install
```

### Compilation

```
make build
```

### Testing

```
make test
```

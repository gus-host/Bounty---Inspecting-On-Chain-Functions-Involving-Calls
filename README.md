# Introduction

**Protocol Name:** Uniswap

**Category:** DeFi

**Smart Contract:** UniswapV2Router02

## Function Analysis

**Function Name:** swapExactTokensForTokens

**Block Explorer Link:** [UniswapV2Router02 Contract on Etherscan](https://etherscan.io/address/0x3e445e3280c5747a188db8d0ab7762838a50e4ff#code)

**Function Code:**
```solidity
function swapExactTokensForTokens(
    uint amountIn,
    uint amountOutMin,
    address[] calldata path,
    address to,
    uint deadline
) external virtual override ensure(deadline) returns (uint[] memory amounts) {
    amounts = UniswapV2Library.getAmountsOut(factory, amountIn, path);
    require(amounts[amounts.length - 1] >= amountOutMin, 'UniswapV2Router: INSUFFICIENT_OUTPUT_AMOUNT');
    TransferHelper.safeTransferFrom(
        path[0], msg.sender, UniswapV2Library.pairFor(factory, path[0], path[1]), amounts[0]
    );
    _swap(amounts, path, to);
}

```

**Used Encoding/Decoding or Call Method:** call

## Explanation
**Purpose:**
The `swapExactTokensForTokens` function facilitates the swapping of a specific amount of one token for another token within the Uniswap V2 protocol. This function ensures that the user receives at least a minimum amount of the output token.

**Detailed Usage:**
In the provided code snippet, the `call` method is not directly visible. However, the `_swap` function, which is called within `swapExactTokensForTokens`, typically uses low-level calls to interact with the Uniswap pairs. Here is a deeper look into the `_swap` function where the low-level `call` method is used:

```solidity
function _swap(uint[] memory amounts, address[] memory path, address _to) internal virtual {
    for (uint i; i < path.length - 1; i++) {
        (address input, address output) = (path[i], path[i + 1]);
        (address token0,) = UniswapV2Library.sortTokens(input, output);
        uint amountOut = amounts[i + 1];
        (uint amount0Out, uint amount1Out) = input == token0 ? (uint(0), amountOut) : (amountOut, uint(0));
        address to = i < path.length - 2 ? UniswapV2Library.pairFor(factory, output, path[i + 2]) : _to;
        IUniswapV2Pair(UniswapV2Library.pairFor(factory, input, output)).swap(
            amount0Out, amount1Out, to, new bytes(0)
        );
    }
}
```

In this `_swap` function, `IUniswapV2Pair(...).swap(...)` indirectly uses the call method to execute the token transfer on the Uniswap pair contract.

**Impact:**
The `swapExactTokensForTokens` function is crucial for the functionality of the Uniswap protocol as it allows users to swap tokens with a guarantee of receiving a minimum amount. The use of low-level calls in the _swap function ensures efficient interaction with the pair contracts, enabling the token swaps to happen seamlessly. This contributes significantly to the protocol's liquidity and user experience by providing reliable and secure token exchange mechanisms.

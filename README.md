# Introduction

**Protocol Name:** Uniswap

**Category:** DeFi

**Smart Contract:** UniswapV2Router02

## Function Analysis

**Function Name:** swapExactTokensForTokens

**Block Explorer Link:** [UniswapV2Router02 Contract on Etherscan](https://etherscan.io/address/0x7a250d5630b4cf539739df2c5dacab1fdd68c4f0#code)

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

**Used Encoding/Decoding or Call Method:** call

Explanation
Purpose:
The swapExactTokensForTokens function facilitates the swapping of a specific amount of one token for another token within the Uniswap V2 protocol. This function ensures that the user receives at least a minimum amount of the output token.

Detailed Usage:
In the provided code snippet, the call method is not directly visible. However, the _swap function, which is called within swapExactTokensForTokens, typically uses low-level calls to interact with the Uniswap pairs. Here is a deeper look into the _swap function where the low-level call method is used:

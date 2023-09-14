# erc-1167-deployer

This is a minimal web app to quickly deploy a [ERC-1167](https://eips.ethereum.org/EIPS/eip-1167) proxy contract.

Here are the opcodes and explanation for the hardcoded `bytecode` in the script:

```
;; Save the first 32 bytes of the ERC-1167 runtime bytecode into memory offset 0
;; (the script will replace bebebe... with the actual contract to proxy to)
PUSH32 0x363d3d373d3d3d363d73bebebebebebebebebebebebebebebebebebebebe5af4
PUSH0
MSTORE

;; Save the remaining 13 bytes of the ERC-1167 runtime bytecode into memory offset 32
;; Note the right-padding to fill 32 bytes is required to align the bytes using MSTORE
PUSH32 0x3d82803e903d91602b57fd5bf300000000000000000000000000000000000000
PUSH1 32
MSTORE

;; Return 45 bytes from memory starting at byte 0
PUSH1 45
PUSH0
RETURN
```

## `PUSH0`

The bytecode above uses the `PUSH0` opcode, which was introduced in the [Shapella upgrade](https://blog.ethereum.org/2023/03/28/shapella-mainnet-announcement) (April 2023). If you're not deploying on Ethereum mainnet, make sure the blockchain you're deploying on supports the `PUSH0` opcode.

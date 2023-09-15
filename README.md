# erc-1167-deployer

This is a minimal web app to quickly deploy a [ERC-1167](https://eips.ethereum.org/EIPS/eip-1167) proxy contract.

Here are the opcodes and explanation for the hardcoded `bytecode` in the script:

```
;; Save the last 13 bytes of the ERC-1167 runtime bytecode into memory offset 13 (implied padding
;; pushes value to offset 32). The bytecode is saved to memory backwards to avoid overwriting the
;; first bytes with MSTORE.
PUSH13 0x3d82803e903d91602b57fd5bf3
PUSH1 13
MSTORE

;; Save the first 32 bytes of the ERC-1167 runtime bytecode into memory offset 0
;; (the JavaScript will replace bebebe... with the actual contract to proxy to).
PUSH32 0x363d3d373d3d3d363d73bebebebebebebebebebebebebebebebebebebebe5af4
PUSH1 0
MSTORE

;; Return 45 bytes from memory starting at byte 0
PUSH1 45
PUSH1 0
RETURN
```

## `PUSH0`

**Caution:** the `PUSH0` opcode was introduced in the [Shapella upgrade](https://blog.ethereum.org/2023/03/28/shapella-mainnet-announcement) (April 2023). If you're not deploying on Ethereum mainnet, make sure the blockchain you're deploying on supports the `PUSH0` opcode.

The ERC-1167 proxy and creation bytecode can be optimized using the `PUSH0` opcode. The optimized opcodes are below (the `PUSH0` optimized minimal proxy is derived from the [Solady's `LibClone.sol` library](https://github.com/Vectorized/solady/blob/main/src/utils/LibClone.sol)):


```
;; Save the last 13 bytes of the Solady minimal PUSH0 proxy bytecode into memory offset 13
;; (implied padding pushes value to offset 32). The bytecode is saved to memory backwards
;; to avoid overwriting the first bytes with MSTORE.
PUSH13 0x5f5f3e6029573d5ffd5b3d5ff3
PUSH1 13
MSTORE

;; Save the first 32 bytes of the Solady minimal PUSH0 proxy bytecode into memory offset 0
;; (the JavaScript will replace bebebe... with the actual contract to proxy to).
PUSH32 0x5f5f365f5f37365f73bebebebebebebebebebebebebebebebebebebebe5af43d
PUSH0
MSTORE

;; Return 45 bytes from memory starting at byte 0
PUSH1 45
PUSH0
RETURN
```

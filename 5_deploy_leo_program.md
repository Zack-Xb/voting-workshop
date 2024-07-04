# \#5. Deployment

## Deploy easily using the Leo Cli
Create a `.env` file with the private key from your wallet account that you funded.

Then run:

```bash
 leo deploy
```

## Request for test tokens from the faucet

Follow [this](https://www.leo.app/blog/aleo-faucet) guide from Leo Wallet.

**Definitely Recommend to install [Leo wallet](https://www.leo.app/), [Puzzle wallet](https://puzzle.online) or [Avail wallet](https://avail.global) if you want to try out Aleo ecosystem!**

If you run into issues with deploying through leo cli or snarkOS cli, follow [this link](https://demo.leo.app) to deploy your Leo program using the interface provided by the Leo Wallet team.

## Test Mint and Transfer Function onchain

Mint Token
```bash
snarkos developer execute \
--broadcast https://api.explorer.aleo.org/v1/testnet/transaction/broadcast \
--private-key <PRIVATEKEY> \
--query https://api.explorer.aleo.org/v1 \
"${PROGRAM_ID}.aleo" mint 100u64
```

Transfer Token
```bash
snarkos developer execute \
--broadcast https://api.explorer.aleo.org/v1/testnet/transaction/broadcast \
--private-key <Private Key> \
--query https://api.explorer.aleo.org/v1 \
"${PROGRAM_ID}.aleo" transfer <recipient_address> 20u64 <token_record>
```

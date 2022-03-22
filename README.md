# uniswapv2-sdk
🛠 An Go SDK for building applications on top of Uniswap V2


```bash

// ##### NETWORK
client, err := ethclient.Dial("rpcNetworkTestnet")
if err != nil {
	panic(err)
}
defer client.Close()

// ##### TRANSACTOPTS
privateKey, err := crypto.HexToECDSA("privateKeyHex")
if err != nil {
	panic(err)
}
chainId, err := client.ChainID(context.Background())
if err != nil {
	panic(err)
}
transactOpts, err := bind.NewKeyedTransactorWithChainID(privateKey, chainId)
if err != nil {
	panic(err)
}

transactOpts.NoSend = false
transactOpts.Value = big.NewInt(99999)
transactOpts.GasPrice = big.NewInt(30000000000)

// ##### CALL FUNCTION
-------------------
router, err := NewUniswapV2Router02(
	common.HexToAddress("routerAddr"),
	client,
)
// WETH
weth, err := router.UniswapV2Router02Caller.WETH(nil)
if err != nil {
	panic(err)
}
fmt.Println(weth)

// ##### TX FUNCTION
tx, err := router.UniswapV2Router02Transactor.SwapExactTokensForTokensSupportingFeeOnTransferTokens(
	transactOpts,
	...,
)
if err != nil {
	panic(err)
}
fmt.Println(tx)

// ##### EVENT FUNCTION
wOpts := new(bind.WatchOpts)
sink := make(chan *NessaNetwork)
subscription, err := router.UniswapV2Router02Filterer.WatchNessaNetwork(wOpts, sink)
if err != nil {
	panic(err)
}
for {
	select {
	case err := <-subscription.Err():
		panic(err)
	case event := <-sink:
		fmt.Println("+evnet: #v", event)
	}
}

```

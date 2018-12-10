## Sūrya's Description Report

### Files Description Table


|  File Name  |  SHA-1 Hash  |
|-------------|--------------|
| MultiAssetProxy.sol | f202291879a5e5da3147012579971e8fa6a12447 |
| MixinAuthorizable.sol | 18626c455ec7644310ab02a429fc74217270689f |
| ../Exchange/MixinAssetProxyDispatcher.sol | 69df94536b6236532d8efc4b4ad3ee7a4f983344 |


### Contracts Description Table


|  Contract  |         Type        |       Bases      |                  |                 |
|:----------:|:-------------------:|:----------------:|:----------------:|:---------------:|
|     └      |  **Function Name**  |  **Visibility**  |  **Mutability**  |  **Modifiers**  |
||||||
| **MultiAssetProxy** | Implementation | MixinAssetProxyDispatcher, MixinAuthorizable |||
| └ | \<Fallback\> | External ❗️ | 🛑  | |
| └ | getProxyId | External ❗️ |   | |
||||||
| **MixinAuthorizable** | Implementation | Ownable, MAuthorizable |||
| └ | addAuthorizedAddress | External ❗️ | 🛑  | onlyOwner |
| └ | removeAuthorizedAddress | External ❗️ | 🛑  | onlyOwner |
| └ | removeAuthorizedAddressAtIndex | External ❗️ | 🛑  | onlyOwner |
| └ | getAuthorizedAddresses | External ❗️ |   | |
||||||
| **MixinAssetProxyDispatcher** | Implementation | Ownable, MAssetProxyDispatcher |||
| └ | registerAssetProxy | External ❗️ | 🛑  | onlyOwner |
| └ | getAssetProxy | External ❗️ |   | |
| └ | dispatchTransferFrom | Internal 🔒 | 🛑  | |


### Legend

|  Symbol  |  Meaning  |
|:--------:|-----------|
|    🛑    | Function can modify state |
|    💵    | Function is payable |

# RoboInu (RBIF) Contracts

### Contract Address: 0x7b32e70e8d73ac87c1b342e063528b2930b15ceb

## Smart Contract
### ERC-20
RoboInu is an ERC-20 contract that implements the following interface:
```Solidity
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
```

### UniSwap Factory and Exchange
RoboInu uses the UniSwap Factory V2 and UniSwap Router v2 contracts to enable swapping tokens for Ether at market prices:

```Solidity
// Uniswap Factory V2 Interface
interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

// Uniswap Router V2 Interface
interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
```

### RoboInu Public Get Methods
The following public getters are available to query:
```Solidity
// Public Parameters
uint256 public _buyTaxFee;
uint256 public _sellTaxFee;
uint256 public _buyLiquidityFee;
uint256 public _sellLiquidityFee;
uint256 public _buymarketingDivisor;
uint256 public _sellmarketingDivisor;
address payable public marketingWallet;
uint256 public _maxTxAmount;
uint256 public _maxTokenHolder;

// Public Get Functions
function balanceOf(address account) public view override returns (uint256)
function numTokensSellToAddToLiquidityAmount() public view returns (uint256)
function totalSupply() public view override returns (uint256)
```

### RoboInu Public Transactions
The following public transaction functions are available to call:
```Solidity
receive() external payable
function isExcludedFromFee(address account) public view returns(bool)
function burn(uint256 amount) external
function transfer(address recipient, uint256 amount) public override returns (bool)
function approve(address spender, uint256 amount) public override returns (bool) 
function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool)
function deliver(uint256 tAmount) public
```

### Constructor
There are three constructor options:

**Mainnet**

This is the constructor deployed to mainnet:

```Solidity
//mainnet

_name = "ROBO INU"; _symbol = "RBIF"; _decimals = 9;
MAX = ~uint256(0); _tTotal = 100000000000 * 10**6 * 10**9; _rTotal = (MAX - (MAX % _tTotal)); // Set Supply
burnAddress = 0x000000000000000000000000000000000000dEaD;                           // Set Burn Address
marketingWallet = payable(0x256bbe11aF79825f8DDe0EC168f58843BAf2a036);              // Set Marketing Wallet Address
uniswapV2Router = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D;                       // Set UniSwap Router V2 Mainnet

```
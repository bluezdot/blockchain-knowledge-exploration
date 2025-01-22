{
blockedList: [
fromAssetSlug___extrinsicType___toAssetSlug
]
}

Transfer thường: cần assetSlug
Transfer xcm: cần fromAssetSlug, toChain
Transfer Nft: cần chainSlug, collectionId
Swap: cần fromAssetSlug, toAssetSlug



```
export interface RequestCheckTransfer extends BaseRequestSign {  
  networkKey: string,  
  from: string,  
  to: string,  
  value?: string,  
  transferAll?: boolean  
  tokenSlug: string  
}

export interface RequestCheckCrossChainTransfer extends BaseRequestSign {  
  originNetworkKey: string,  
  destinationNetworkKey: string,  
  from: string,  
  to: string,  
  transferAll?: boolean,  
  value: string,  
  tokenSlug: string,  
  showExtraWarning?: boolean  
}

export interface NftTransactionRequest {  
  networkKey: string,  
  senderAddress: string,  
  recipientAddress: string,  
  
  nftItemName?: string, // Use for confirmation view only  
  params: Record<string, any>,  
  nftItem: NftItem  
}

export interface ChainflipSwapTxData extends SwapBaseTxData {  
  depositChannelId: string;  
  depositAddress: string;  
  estimatedDepositChannelExpiryTime?: number;  
}

export interface HydradxSwapTxData extends SwapBaseTxData {  
  txHex: string;  
}

export interface SwapBaseTxData {  
  provider: SwapProvider;  
  quote: SwapQuote;  
  address: string;  
  slippage: number;  
  recipient?: string;  
  process: CommonOptimalPath;  
}
```

```
export interface StakePoolingBondingParams extends BaseRequestSign {  
  nominatorMetadata?: NominatorMetadata,  
  chain: string,  
  selectedPool: NominationPoolInfo,  
  amount: string,  
  address: string  
}

export interface YieldLeaveParams extends BaseRequestSign {
  address: string;
  amount: string;
  slug: string;
  selectedTarget?: string;
  fastLeave: boolean;
  poolInfo: YieldPoolInfo;  
}

export interface UnbondingSubmitParams extends BaseRequestSign {  
  amount: string;  
  chain: string;  
  nominatorMetadata: NominatorMetadata;
  validatorAddress?: string;  
  isLiquidStaking?: boolean;  
  derivativeTokenInfo?: _ChainAsset;  
  exchangeRate?: number;  
  inputTokenInfo?: _ChainAsset;  
  isFastUnbond: boolean;  
}

export interface StakeClaimRewardParams extends BaseRequestSign {  
  address: string;  
  slug: string;  
  unclaimedReward?: string;  
  bondReward?: boolean;  
}

export interface YieldWithdrawalParams extends BaseRequestSign {  
  address: string;
  slug: string;  
  unstakingInfo: UnstakingInfo;  
}

export interface TuringStakeCompoundParams extends BaseRequestSign {  
  address: string,  
  collatorAddress: string,  
  networkKey: string,  
  accountMinimum: string,  
  bondedAmount: string,  
}

export interface TuringCancelStakeCompoundParams extends BaseRequestSign {  
  taskId: string;  
  networkKey: string;  
  address: string;  
}

export interface StakeCancelWithdrawalParams extends BaseRequestSign {  
  address: string;  
  slug: string;  
  selectedUnstaking: UnstakingInfo;  
}

export interface HandleYieldStepParams extends BaseRequestSign {  
  path: OptimalYieldPath;  
  data: SubmitYieldJoinData;  
  currentStep: number;  
}

export interface SubmitYieldStepData extends AbstractSubmitYieldJoinData {
  exchangeRate: number,
  inputTokenSlug: string,  
  derivativeTokenSlug?: string,  
  rewardTokenSlug: string,  
  feeTokenSlug: string  
}
export interface AbstractSubmitYieldJoinData {  
  slug: string;  
  amount: string;  
  address: string;  
}

export interface TokenSpendingApprovalParams {  
  chain: string;  
  contractAddress: _Address;  
  spenderAddress: _Address;  
  owner: _Address;  
  amount?: string;  
}

export interface TransactionConfig {  
    from?: string | number;  
    to?: string;  
    value?: number | string | BN;  
    gas?: number | string;  
    gasPrice?: number | string | BN;  
    maxPriorityFeePerGas?: number | string | BN;  
    maxFeePerGas?: number | string | BN;  
    data?: string;  
    nonce?: number;  
    chainId?: number;  
    common?: Common;  
    chain?: string;  
    hardfork?: string;  
}
```
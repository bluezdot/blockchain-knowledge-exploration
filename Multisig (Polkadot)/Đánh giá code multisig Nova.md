#### Phân tích Logic Multisig trong Nova Spektr
## Tổng quan

  

Hệ thống multisig trong Nova Spektr hỗ trợ hai loại:

1. **Multisig thông thường (Regular Multisig)**: Sử dụng pallet multisig trực tiếp

2. **Flexible Multisig**: Kết hợp multisig với proxy để linh hoạt hơn trong việc thay đổi signatories

  

---

  

## Cấu trúc chính

  

### 1. **Entities** (`src/renderer/entities/multisig-accounts/`)

  

#### `api/multisigsService.ts`

- **Chức năng chính**: Tìm và lọc multisig accounts

- **Các hàm quan trọng**:

- `filterMultisigsAccounts()`: Lọc multisig accounts từ GraphQL query

- `findFlexibleMultisigs()`: Tìm flexible multisigs bằng cách kiểm tra proxy entries

- `getUniqMultisigs()`: Loại bỏ multisigs trùng lặp

  

#### `lib/mulitisigs-utils.ts`

- `isFlexibleMultisigSupported()`: Kiểm tra chain có hỗ trợ flexible multisig

- `buildMultisigAccount()`: Tạo MultisigAccount object từ thông tin cơ bản

  

#### `lib/multisigTx/common/utils.ts`

- `getPendingMultisigTxs()`: Lấy các multisig transactions đang pending từ chain

- `buildMultisigTx()`: Build MultisigOperation từ transaction data

  

### 2. **Domains** (`src/renderer/domains/network/multisig-operation/`)

  

#### `service.ts`

**Service chính xử lý logic multisig operations:**

  

- **`sortSignatories()`**: Sắp xếp signatories theo thứ tự byte (rất quan trọng!)

- Public keys được so sánh byte-for-byte và sắp xếp tăng dần

- Thứ tự signatories phải đúng, nếu không transaction sẽ fail

  

- **`getMultisigAccountId()`**: Tạo multisig account ID từ signatories và threshold

- Sử dụng `createKeyMulti()` từ `@polkadot/util-crypto`

- Hỗ trợ cả SR25519 và Ethereum (truncate to 20 bytes)

  

- **`getOtherSignatories()`**: Lấy danh sách signatories khác (loại trừ signer hiện tại)

- Đảm bảo thứ tự đúng

  

- **`getOperationId()`**: Tạo unique ID cho operation

- Format: `${chainId}-${callHash}-${accountId}-${block}-${index}`

  

- **`getTransactionFromChain()`**: Lấy call data từ chain khi không có trong indexer

- Đọc từ block hash và tìm inner call trong extrinsic

  

#### `store.ts`

**Effector store quản lý state của multisig operations:**

- `$list`: Danh sách tất cả multisig operations

- `populateFx`: Load operations từ storage

- `addOperationsFx`: Thêm operations mới

- `updateOperationsFx`: Cập nhật operations

- `updateCallDataFx`: Cập nhật call data khi decode được

- `subscribeResource`: Subscribe để sync operations từ SubQuery

- `subscribeEventsResource`: Subscribe events (approve/reject)

  

### 3. **Features**

  

#### `features/multisig-wallet/` - Feature chính cho multisig wallet

**File `index.tsx`:**

- **Account SDK integration**:

- `availableOnChain()`: Kiểm tra chain hỗ trợ multisig

- `collectAccountChildren()`: Thu thập signatory accounts như children

- `validateRouteBalances()`: Validate balance cho multisig deposit

  

- **Transaction SDK integration**:

- `encode()`: Encode multisig transaction

- `decode()`: Decode multisig transaction

- `wrap()`: Wrap transaction thành `asMulti` call

- **Regular multisig**: Wrap trực tiếp

- **Flexible multisig**: Wrap qua proxy trước, rồi mới wrap multisig

- `unwrap()`: Unwrap để lấy inner call

  

#### `features/multisig-operations/` - Quản lý operations

**File `model/approve-model.ts`:**

- Quản lý flow approve multisig operation

- Tính toán weight, fee, deposit

- Validate transaction trước khi submit

  

**File `model/reject-model.ts`:**

- Quản lý flow reject multisig operation

- Build reject transaction

  

### 4. **Pallet** (`src/renderer/shared/pallet/multisig/`)

  

#### `storage.ts`

- `multisigs()`: Query multisigs từ chain storage

- Returns: Map of `<AccountId, CallHash>` -> `Multisig` data

  

#### `schema.ts`

- Định nghĩa types:

- `MultisigTimepoint`: `{ height, index }`

- `Multisig`: `{ when, deposit, depositor, approvals }`

  

#### `consts.ts`

- `depositBase()`: Base deposit cần để tạo multisig

- `depositFactor()`: Deposit per threshold

- `maxSignatories()`: Số lượng signatories tối đa

  

### 5. **Aggregates** (`src/renderer/aggregates/selected-wallet-multisig-operations/`)

  

#### `model.ts`

- Filter multisig operations cho wallet đang được chọn

- Xử lý flexible multisig: Chỉ hiển thị operations có proxy call matching

  

---

  

## Flow hoạt động

  

### 1. **Tạo Multisig Wallet**

  

**Regular Multisig:**

1. User chọn signatories và threshold

2. System tính toán multisig account ID (sắp xếp signatories đúng thứ tự!)

3. Lưu wallet vào storage

  

**Flexible Multisig:**

1. Tương tự Regular Multisig

2. **Thêm bước**: Tạo proxy account với proxy type `Any`

3. Multisig account được set làm delegate của proxy

4. User sử dụng proxy account address

  

### 2. **Tạo Multisig Transaction**

  

**Flow:**

```

User Transaction

→ wrap()

→ Tìm signatory trong route

→ Tính toán otherSignatories (đã sắp xếp)

→ Tính maxWeight

→ Wrap thành asMulti call

```

  

**Đối với Flexible Multisig:**

```

User Transaction

→ Wrap thành proxy.proxy call (real = multisig account)

→ Wrap proxy call thành multisig.asMulti call

```

  

### 3. **Approve/Reject Operation**

  

**Approve:**

1. Load pending operation từ storage

2. Kiểm tra call data (nếu chưa có thì fetch từ chain)

3. Chọn signatory để approve

4. Build transaction:

- **Nếu có call data**: `MULTISIG_AS_MULTI` (execute luôn)

- **Nếu không có call data**: `MULTISIG_APPROVE_AS_MULTI` (chỉ approve, chưa execute)

5. Build với `maybeTimepoint` (nếu đã có approvals trước đó)

6. Sign và submit

  

**Chi tiết `buildApproveMultisigTx()`:**

```typescript

{

type: hasCallData ? MULTISIG_AS_MULTI : MULTISIG_APPROVE_AS_MULTI,

args: {

threshold,

otherSignatories, // Đã được sắp xếp

maxWeight,

maybeTimepoint: { height, index }, // Từ operation đầu tiên

call: callData, // Nếu có

callHash,

}

}

```

  

**Reject:**

1. Build `cancelAsMulti` transaction

2. Sử dụng timepoint từ operation đầu tiên

3. Sign và submit

  

**Chi tiết `buildRejectMultisigTx()`:**

```typescript

{

type: MULTISIG_CANCEL_AS_MULTI,

args: {

threshold,

otherSignatories,

callHash,

maybeTimepoint: { height, index }, // Từ operation

}

}

```

  

### 4. **Sync Operations**

  

**Từ SubQuery:**

- Subscribe multisig operations

- Subscribe events (approve/reject)

- Merge với local state

  

**Từ Chain:**

- Query `multisig.multisigs` storage để lấy pending transactions

- Fetch call data từ chain nếu cần

  

---

  

## Các điểm quan trọng

  

### 1. **Thứ tự Signatories**

⚠️ **Rất quan trọng**: Signatories PHẢI được sắp xếp theo thứ tự byte trước khi:

- Tạo multisig account ID

- Build `asMulti` transaction

  

Nếu thứ tự sai → Transaction sẽ fail!

  

### 2. **Multisig Deposit**

- **Deposit** = `depositBase + (depositFactor * threshold)`

- Deposit được refund khi operation complete/cancel

- Phải reserve deposit từ signatory account

  

### 3. **Timepoint**

- Được tạo khi operation đầu tiên được approve

- Format: `{ height: blockNumber, index: extrinsicIndex }`

- Cần thiết cho các approve tiếp theo

  

### 4. **Weight**

- Cần tính toán `maxWeight` cho `asMulti` call

- Sử dụng `transactionService.getExtrinsicWeight()` hoặc `api.tx.multisig.asMulti().paymentInfo()`

  

### 5. **Flexible Multisig vs Regular**

- **Regular**: Multisig account = address công khai

- **Flexible**: Proxy account = address công khai, Multisig account = delegate

- Flexible cho phép thay đổi signatories mà không cần tạo address mới

  

---

  

## Key Files Reference

  

### Core Logic

- `domains/network/multisig-operation/service.ts` - Core service

- `domains/network/multisig-operation/store.ts` - State management

- `entities/multisig-accounts/api/multisigsService.ts` - Multisig discovery

- `entities/transaction/lib/transactionBuilder.ts` - Build approve/reject transactions

- `shared/pallet/multisig/` - Chain interaction

- `shared/transactions/createMultisigDeposit.ts` - Tính toán multisig deposit

  

### Features

- `features/multisig-wallet/index.tsx` - Multisig wallet feature

- `features/multisig-operations/` - Operations management

- `features/multisig-wallet-create/` - Wallet creation UI

  

### UI Components

- `widgets/Transfer/` - Transfer với multisig support

- `widgets/Staking/` - Staking operations với multisig

  

---

  

## Testing

  

Có các test files:

- `tests/system/tests/onboarding/multisig.onboarding.system.test.ts`

- `tests/system/tests/transfers/ms.transfers.system.test.ts`

- `domains/network/multisig-operation/service.test.ts`

  

---

  

## Tính toán Multisig Deposit

  

**Công thức:**

```typescript

deposit = depositBase + (depositFactor * threshold)

```

  

- `depositBase`: Base deposit từ chain consts

- `depositFactor`: Deposit per threshold từ chain consts

- `threshold`: Số lượng signatures cần thiết

  

**Ví dụ:** Nếu `depositBase = 100`, `depositFactor = 50`, `threshold = 3`:

- `deposit = 100 + (50 * 3) = 250`

Deposit này được refund khi operation complete hoặc cancel.

  

---

  

## Types và Interfaces

  

### MultisigAccount

```typescript

{

accountType: AccountType.MULTISIG,

accountId: AccountId,

threshold: number,

signatories: Signatory[],

name: string,

cryptoType: CryptoType,

signingType: SigningType.MULTISIG,

}

```

  

### FlexibleMultisigAccount

```typescript

{

...MultisigAccount,

accountType: AccountType.FLEXIBLE_MULTISIG,

multisigAccountId: AccountId, // Multisig account ID

accountId: AccountId, // Proxy account ID

proxied: {

proxyAccountId: AccountId,

accountId: AccountId,

proxyType: 'Any',

delay: number,

}

}

```

  

### MultisigOperation

```typescript

{

id: string, // `${chainId}-${callHash}-${accountId}-${block}-${index}`

status: 'pending' | 'completed' | 'cancelled',

section: string,

method: string,

accountId: AccountId,

chainId: ChainId,

depositor: AccountId,

transaction: DecodedTransaction,

callHash: CallHash,

callData: HexString | null,

blockCreated: number,

indexCreated: number,

timestamp: number,

events: MultisigEvent[],

}

```

  

### MultisigEvent

```typescript

{

id: string, // `${operationId}-${signer}-${status}`

accountId: AccountId,

extrinsicHash: HexString,

blockCreated: number,

indexCreated: number,

timestamp: number,

status: 'approve' | 'reject',

}

```

  

---

  

## Notes

  

1. GraphQL query (`FILTER_MULTISIG_ACCOUNT_IDS`) được sử dụng để tìm multisigs từ SubQuery indexer

2. System hỗ trợ cả Ethereum và Substrate multisigs

3. Multisig operations được lưu trong IndexedDB (Dexie)

4. Real-time sync qua SubQuery subscriptions

5. Call data có thể được fetch từ chain nếu không có trong indexer

6. Weight được tính toán để đảm bảo transaction có đủ gas
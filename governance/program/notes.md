## Addresses

- [`GovER5Lthms3bLBqWub97yVrMmEogzX7xNjdXpPPCVZw`](https://explorer.solana.com/address/GovER5Lthms3bLBqWub97yVrMmEogzX7xNjdXpPPCVZw) default _spl-governance_ instance
- [`GTesTBiEWE32WHXXE2S4XbZvA5CrEc4xs6ZgRe895dP`](https://explorer.solana.com/address/GTesTBiEWE32WHXXE2S4XbZvA5CrEc4xs6ZgRe895dP) test instance which can be used to setup test DAOs
- [`GqTPL6qRf5aUuqscLh8Rg2HTxPUXfhhAXDptTLhp1t2J`](https://explorer.solana.com/address/GqTPL6qRf5aUuqscLh8Rg2HTxPUXfhhAXDptTLhp1t2J) Mango Governance Token

## TL;DR

It was determined that discriminating accounts based on size and pattern only will not work for
this program. However the first byte of each account holds an `account_type` discriminator
which maps to [this
enum](https://docs.rs/spl-governance/3.1.1/spl_governance/state/enums/enum.GovernanceAccountType.html).
We will thus provide a somewhat custom implementation that discriminates based on that first
byte.

Additionally [here is a JSON
file](https://github.com/solana-labs/governance-ui/blob/329513508c777e90b3e17787379d85911cc3d60c/public/realms/mainnet-beta.json) that might help us find all deployed governance programs.

## Accounts

### GovernanceV2

- max-size: `236`

### NativeTreasury

- max-size: `0` (has no data)

### ProgramMetadata

- max-size: `88`

### ProposalDeposit

- max-size: `1 + 32 + 32 + 64 = 129`

### ProposalTransactionV2

- max-size: `instructions_size + 62` (`instructions: Vec<InstructionData>`)

### ProposalV2

- `max-size`: `name_len + description_link_len + options_size + 297)
  - options_size: `(19 + label_len) * options)`

### RealmV2

- max-size: `self.name.len() + 264`

### RealmConfigAccount

- max-size: `1 + 32 + (75 * 2) + 110 = 293`
- size: `1 + 32 + 75 + 75 + 110 = 293`

#### SubSize

- `GoverningTokenConfig`: `(1 + 32) + (1 + 32) + 1 + 8 = 75`

### RequiredSignatory

- max-size: _unknown_
- size: `1 + 1 + 32 + 32 = 66`

### SignatoryRecordV2

- max-size: _unknown_, instance size should be used
- size: `1 + 32 + 32 + 1 + 8 = 74`

### TokenOwnerRecordV2

- max-size: `282`

### VoteRecordV2

- max-size: _unknown_, instance size should be used
- size: `1 + 32 + 32 + 1 + 64 + vote + 8 = 138 + vote`

### SubSize

- `Vote`: `1 | 1 + (vote_choice_len * 2)`

## Sizes

```
==   0    NativeTreasury
==  66    RequiredSignatory
==  88    ProgramMetadata
== 129    ProposalDeposit
== 282    TokenOwnerRecordV2
== 293    RealmConfigAccount

>= 138    VoteRecordV2
>= 264    RealmV2
>= 316    ProposalV2
?? ???    SignatoryRecordV2
```

### Min Sizes Determined by Chainsaw from IDL fields

```
== 49     ProposalTransactionV2
== 65     ProgramMetadata
== 66     RequiredSignatory
== 74     SignatoryRecordV2
== 82     VoteRecordV2
== 129    ProposalDeposit
== 143    RealmConfigAccount
== 169    RealmV2
== 174    ProposalV2
== 197    GovernanceV2
== 249    TokenOwnerRecordV2
```

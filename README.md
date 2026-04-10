# WolfPack CLI

CLI tool to interact with the **WolfPack (WP)** smart contract on Qubic (Contract Index: **27**).

Based on [qubic-cli](https://github.com/qubic/qubic-cli).

---

## BUILD

```bash
git submodule update --init --recursive
mkdir build && cd build
cmake ../
cmake --build .
```

---

## BASIC CONFIG

| Option | Description |
|---|---|
| `-nodeip <IP>` | IP address of the Qubic node (default: `127.0.0.1`) |
| `-nodeport <PORT>` | Port of the Qubic node (default: `21841`) |
| `-seed <SEED>` | 55-char lowercase seed (required for write commands) |
| `-scheduletick <OFFSET>` | Tick offset for scheduling transactions (default: `20`) |

> **Important:** `-seed` must come **before** the command in the argument list.

---

## WOLFPACK COMMANDS

### Query Commands (read-only)

#### Get Contract Status

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -wpstatus
```

Returns: holder count, total tokens snapshot, clan member count, pending revenue, reinvestment fund, total distributed/deposited, last payout tick, last distribution epoch, admin address.

#### Get Holder Info

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -wpholderinfo <ADDRESS>
```

Returns: token balance and holder status (YES/NO) for the given address.

#### Get Clan Member Info

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -wpclaninfo <ADDRESS>
```

Returns: rank (0–4) and membership status (YES/NO) for the given address.

---

### Write Commands (require `-seed`)

#### Deposit Revenue

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -seed <SEED> -wpdeposit <AMOUNT>
```

Deposit `<AMOUNT>` QU as revenue into the WolfPack contract. Revenue is automatically distributed at 11:00 UTC:
- **70%** → WP token holders (proportional)
- **10%** → SC shareholders (burn)
- **10%** → Clan members (rank-weighted)
- **10%** → Reinvestment fund

#### Add Clan Member (admin-only)

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -seed <SEED> -wpaddclan <ADDRESS> <RANK>
```

Add a clan member with the given rank (0–4). Rank multipliers:
| Rank | Multiplier |
|------|-----------|
| 0 | 1.0x |
| 1 | 1.2x |
| 2 | 1.5x |
| 3 | 2.0x |
| 4 | 3.0x |

#### Remove Clan Member (admin-only)

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -seed <SEED> -wpremoveclan <ADDRESS>
```

#### Set Clan Rank (admin-only)

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -seed <SEED> -wpsetclanrank <ADDRESS> <RANK>
```

Update a clan member's rank (0–4).

#### Set Admin (admin-only)

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -seed <SEED> -wpsetadmin <ADDRESS>
```

Transfer admin rights to a new address.

#### Set Exclude Address (admin-only)

```bash
./qubic-cli -nodeip <IP> -nodeport <PORT> -seed <SEED> -wpsetexclude <SLOT> <ADDRESS>
```

Set an address to exclude from token holder distributions. Slot `1` or `2`.

---

## EXAMPLES

```bash
# Check contract status on testnet
./qubic-cli -nodeip 135.181.160.185 -nodeport 31841 -wpstatus

# Check if an address holds WP tokens
./qubic-cli -nodeip 135.181.160.185 -nodeport 31841 -wpholderinfo ABCDEF...XYZ

# Deposit 1000 QU revenue
./qubic-cli -nodeip 135.181.160.185 -nodeport 31841 -seed <your-seed> -wpdeposit 1000

# Add clan member with rank 2
./qubic-cli -nodeip 135.181.160.185 -nodeport 31841 -seed <your-seed> -wpaddclan ABCDEF...XYZ 2
```

---

## LICENSE

See [LICENSE.md](LICENSE.md).

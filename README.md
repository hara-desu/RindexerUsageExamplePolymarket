# 📘 Running rindexer on Polymarket

---

# ✅ Prerequisites

Before using **rindexer**, ensure your system meets the following requirements:

### **System Requirements**

- **OS:** macOS, Linux, or Windows (WSL recommended for Windows)
- **Tools Needed:**
  - `curl`
  - `bash`
  - `git` (optional but useful)
  - Access to a fast and reliable **Polygon RPC** (Alchemy recommended)

### **Blockchain Requirements**

- An **Alchemy API key** for Polygon Mainnet
- Contract ABIs for the Polymarket contracts you want to index

---

# 🛠 Installing rindexer

Install rindexer using the official installation script:

```bash
curl -L https://rindexer.xyz/install.sh | bash
```

Verify installation:

```bash
rindexer --version
```

Windows users should run the installation under **WSL**.

---

# Running rindexer on Polymarket

## 🔧 Overview

This guide walks you through how to configure and run **rindexer** to index Polymarket smart-contract events, starting from the current block and exporting data to CSV.

---

## 1. Get an Alchemy API Key

Create an Alchemy account and obtain an API key for **Polygon Mainnet**.

Example RPC URL:

```
https://polygon-mainnet.g.alchemy.com/v2/<YOUR_KEY>
```

Store it in a `.env` file:

```
ALCHEMY_RPC=https://polygon-mainnet.g.alchemy.com/v2/<YOUR_KEY>
```

Load the `.env` values into your shell:

```bash
export $(grep -v '^#' .env | xargs)
```

---

## 2. Identify Relevant Polymarket Contracts

Commonly used Polymarket smart contracts:

- **Main Polymarket / CTF Contract**  
  `0x4d97dcd97ec945f40cf65f87097ace5ea0476045`

- **CTF Exchange – Current (NegRisk Exchange)**  
  `0xC5d563A36AE78145C45a50134d48A1215220f80a`

- **CTF Exchange – Legacy**  
  `0x4bFb41d5B3570DeFd03C39a9A4D8dE6Bd8B8982E`

- **UMA Adapter (Oracle Bridge)**  
  `0x65070BE91477460D8A7AeEb94ef92fe056C2f2A7`

- **USDC (Collateral Token)**  
  `0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174`

To decode events:

1. Open the contract page on Polygonscan
2. Go to **Contract → Code → ABI**
3. Copy the ABI into a `.abi.json` file

---

## 3. Configure rindexer

Create a new rindexer project:

```bash
rindexer new no-code
```

### ✔ Choose the contract

Select which Polymarket contract you want to index.

### ✔ Get the latest block number

Visit:  
https://polygonscan.com

Example:

```
79213501
```

Use this in your rindexer configuration:

```
start_block: "79213501"
```

---

### ✔ Example configuration steps

- Edit `rindexer.yaml`
- Add:
  - network = polygon
  - rpc = `${ALCHEMY_RPC}`
  - contract address(es)
  - ABI file path
  - starting block

Start the indexer:

```bash
rindexer start indexer
```

---

### ✔ Restarting the indexer

To start fresh:

```bash
rm -rf generated_csv/*
rindexer start indexer
```

---

### ⚠ RPC Rate Limits

If you see:

```
RPC RATE LIMITED (HTTP 429)
```

It means the free Alchemy tier is insufficient.  
A **paid Alchemy plan (Growth or higher)** is recommended.

---

## 4. Run rindexer

rindexer will:

1. Index blocks from the `start_block`
2. Catch up to the chain head
3. Switch into **live indexing mode**

CSV data will appear in:

```
generated_csv/
```

---

## 5. Interpreting CSV Event Fields

To understand an event:

1. Go to the contract page on Polygonscan
2. Open **Contract → Code**
3. Check the event definitions
4. Match ABI event parameters with CSV fields

---

## ✅ Summary

You now know how to:

- Install rindexer
- Set up environment variables
- Configure rindexer for Polymarket
- Start and restart indexing
- Handle RPC rate limits
- Interpret CSV outputs

---

# 🇰🇷 Polymarket용 rindexer 실행 가이드 (한국어)

---

# ✅ 사전 준비 사항

### **시스템 요구 사항**

- macOS / Linux / Windows(WSL 권장)
- 필수 도구:
  - `curl`
  - `bash`
  - `git`(선택)
  - 빠르고 안정적인 **Polygon RPC**

### **블록체인 요구 사항**

- Alchemy Polygon Mainnet API 키
- 인덱싱할 Polymarket 컨트랙트 ABI

---

# 🛠 rindexer 설치

공식 스크립트로 설치:

```bash
curl -L https://rindexer.xyz/install.sh | bash
```

확인:

```bash
rindexer --version
```

Windows는 WSL 사용 권장.

---

# 🔧 개요

이 문서는 Polymarket 스마트컨트랙트 이벤트를 rindexer로 인덱싱하여 CSV로 저장하는 방법을 설명합니다.

---

## 1. Alchemy API 키 발급

`.env` 파일에 저장:

```
ALCHEMY_RPC=https://polygon-mainnet.g.alchemy.com/v2/<YOUR_KEY>
```

환경 변수 로드:

```bash
export $(grep -v '^#' .env | xargs)
```

---

## 2. 필요한 Polymarket 컨트랙트

- 메인 CTF 컨트랙트  
  `0x4d97dcd97ec945f40cf65f87097ace5ea0476045`

- 최신 CTF Exchange  
  `0xC5d563A36AE78145C45a50134d48A1215220f80a`

- 레거시 CTF Exchange  
  `0x4bFb41d5B3570DeFd03C39a9A4D8dE6Bd8B8982E`

- UMA Adapter  
  `0x65070BE91477460D8A7AeEb94ef92fe056C2f2A7`

- USDC  
  `0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174`

ABI는 Polygonscan → Contract → Code → ABI에서 복사.

---

## 3. rindexer 설정

새 프로젝트 생성:

```bash
rindexer new no-code
```

### ✔ 최신 블록 번호 확인

https://polygonscan.com  
예: `79213501`

`start_block`에 입력.

---

### ✔ rindexer.yaml 설정

- network = polygon
- rpc = `${ALCHEMY_RPC}`
- 컨트랙트 주소
- ABI 파일 위치
- 시작 블록

---

### ✔ 인덱서 실행

```bash
rindexer start indexer
```

---

### ✔ 인덱서 초기화

```bash
rm -rf generated_csv/*
rindexer start indexer
```

---

### ⚠ RPC 제한

오류 메시지:

```
RPC RATE LIMITED (HTTP 429)
```

→ 무료 Alchemy RPC로는 인덱싱 불가  
→ **유료 플랜** 필요

---

## 4. CSV 생성

CSV는:

```
generated_csv/
```

폴더에 생성됨.

---

## 5. CSV 필드 해석

- Polygonscan 컨트랙트 페이지
- Contract → Code
- 이벤트 정의 확인
- CSV 컬럼과 비교

---

## 🎉 요약

- rindexer 설치
- 환경 변수 사용
- Polymarket ABI 설정
- 실시간 인덱싱
- RPC 제한 해결
- CSV 해석 가능

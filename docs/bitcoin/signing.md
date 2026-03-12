# 비트코인 서명 (Signing)

## 개요
트랜잭션 서명�� 소유자가 해당 UTXO를 소비할 권한이 있음을 증명하는 과정입니다. 전통적으로 ECDSA(Elliptic Curve Digital Signature Algorithm, secp256k1)를 사용합니다. 최근에는 Taproot/Schnorr(BIP-340) 같은 발전이 있습니다.

## 서명 대상 (SIGHASH)
- 서명은 트랜잭션의 특정 부분들에 대해 해시를 취한 값에 대해 이뤄집니다.
- SIGHASH 타입: ALL, NONE, SINGLE, + ANYONECANPAY 옵션 — 각 옵션은 서명에 포함되는 입력/출력 범위를 결정.

## Legacy 서명과 SegWit 서명(BIP-143)
- Legacy: 전체 트랜잭션(및 스크립트 재구성)에 대한 해시를 만들어 ECDSA 서명.
- SegWit(BIP-143): 효율적이고 안전한 서명 해시 계산 방식 도입(스크립트 혼동 및 서명중복 문제 완화).

## 서명 데이터 위치
- Legacy: scriptSig 필드에 서명이 삽입되어 스크립트와 결합되어 평가됨.
- SegWit: 서명은 witness 데이터(별도 블록 영역)에 저장되어 txid에 영향을 주지 않음.

## ECDSA 개요
- 개인키로 메시지 해시에 대해 서명 → (r, s) 값.
- 검증자는 공개키와 서명값으로 유효성 검사.

## Schnorr / Taproot (요약)
- Schnorr 서명(BIP-340): 단일합성 서명(aggregate), 더 간단한 보안 증명, 키/스크립트 합성에 유리.
- Taproot(BIP-341/BIP-342): 스크립트 및 키 유연성 증가, 프라이버시 향상.

## 서명 생성 흐름(간단)
1. 소비할 UTXO의 scriptPubKey와 트랜잭션을 기반으로 SIGHASH 해시를 계산.
2. 해당 해시에 대해 개인키로 서명(ECDSA 또는 Schnorr).
3. 서명을 scriptSig(legacy) 또는 witness(segwit)에 배치.
4. 네트워크에 전파 후 포함 여부를 기다림.

## 보안 권장사항
- 개인키 절대 노출 금지.
- 서명 전 검증용으로 트랜잭션 내용(수신 주소, 금액, 수수료)을 반드시 확인.
- 하드웨어 월렛 사용 권장(특히 고액의 경우).

## 참고 BIP
- BIP-143 (SegWit 서명 해시), BIP-340 (Schnorr), BIP-341/342 (Taproot)

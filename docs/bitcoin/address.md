# 비트코인 주소 (Bitcoin Addresses)

## 개요
비트코인 주소는 송금 대상(수신자)을 식별하는 식별자입니다. 일반적으로 공개키(또는 공개키 해시)를 사람이 읽을 수 있는 문자열로 인코딩합니다. 주소 형식에 따라 사용법과 기능이 다릅니다.

## 주요 주소 유형
- P2PKH (Pay-to-PubKeyHash)
  - 접두사: `1`
  - 인코딩: Base58Check
  - 설명: 공개키 해시(RIPEMD160(SHA256(pubkey)))를 사용한 전통적 주소
  - 예: `1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa`

- P2SH (Pay-to-ScriptHash)
  - 접두사: `3`
  - 인코딩: Base58Check
  - 설명: 스크립트를 해시한 값으로 지불. 멀티시그 및 복잡한 스크립트에서 사용
  - 예: `3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy`

- Bech32 (SegWit: P2WPKH / P2WSH)
  - 접두사: `bc1` (메인넷)
  - 인코딩: Bech32
  - 설명: 세그윗 적용 주소, 더 낮은 수수료(트랜잭션 가중치 감소) 및 향상된 오류 검출
  - 예: `bc1qw508d6qejxtdg4y5r3zarvary0c5xw7kygt080`

## 주소 생성 요약
1. 개인키(private key)를 생성.
2. 개인키로부터 공개키(public key)를 파생.
3. 공개키에 해시를 적용(예: SHA256 → RIPEMD160) — P2PKH/P2WPKH의 페이로드.
4. 주소 형식에 따라 Base58Check 또는 Bech32로 인코딩.

## 체크섬과 인코딩
- Base58Check: 버전바이트 + 페이로드 + 4바이트 체크섬 → Base58 변환.
- Bech32: HRP(휴먼리더블파트) + 페이로드 + 6문자 체크섬. 대소문자 구별 규칙 있음.

## HD 지갑과 xpub
- BIP32/BIP44 등으로 계층적 결정적(HD) 지갑이 가능.
- xprv/xpub(확장키)를 통해 여러 주소를 파생. xpub만 있으면 주소 생성 가능(개인키 노출 없이).

## 보안/모범 사례
- 주소 재사용을 피하기(프라이버시 향상).
- 개인키와 시드를 오프라인 백업 및 안전하게 보관.
- 가능하면 Bech32(segwit) 주소 사용 권장.

## 참고 BIP
- BIP-32 (HD wallets), BIP-44 (multi-account), BIP-173 (Bech32)

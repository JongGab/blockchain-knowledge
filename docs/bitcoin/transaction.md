# 비트코인 트랜잭션 (Bitcoin Transactions)

## 개요
트랜잭션은 UTXO(미사용 트랜잭션 출력)를 소비하고 새로운 UTXO��� 생성하는 기본 단위입니다. 네트워크는 트랜잭션을 통해 가치(코인)를 이동합니다.

## 구조(기본 필드)
- 버전(version)
- 입력 리스트(inputs)
  - 이전 트랜잭션 해시(txid)
  - 출력 인덱스(vout)
  - scriptSig (혹은 witness 참조)
  - sequence
- 출력 리스트(outputs)
  - value (satoshi)
  - scriptPubKey
- locktime

## 입력(input)
- 각 입력은 이전 트랜잭션의 특정 출력(UTXO)을 참조하여 소비.
- scriptSig(비세그윗 이전): 서명 및 공개키를 포함하여 스크립트를 만족시킴.
- 세그윗 포함 트랜잭션은 witness에 서명 정보를 둠.

## 출력(output)
- value: 전송할 사토시 단위 금액.
- scriptPubKey: 수신자가 자금을 소비하기 위해 만족시켜야 하는 스크립트(예: P2PKH, P2SH, P2WPKH).

## 트랜잭션 ID (txid) / wtxid
- txid: 전통적(legacy) 직렬화의 더블 SHA256 해시.
- 세그윗 도입 후, witness 포함 전체 직렬화를 해시한 값이 wtxid(또는 txid와 구분)로 사용될 수 있음.

## 직렬화와 사이즈/가중치
- 트랜잭션 가중치(weight) = base_size * 3 + total_size (BIP141 기준).
- 수수료 = (수수료 비율, sat/vbyte) * vsize(virtual size).
- 세그윗으로 vsize가 줄어 수수료 절감 가능.

## 수수료 계산
- 수수료 = 입력(모든 소비 UTXO 값) - 출력(모든 생성 UTXO 값).
- 채굴자 우선순위는 수수료/바이트 비율이나 더 복잡한 정책에 따름.

## 스크립트와 실행
- 비트코인 스크립트는 스택 기반의 작은 언어.
- 표준 스크립트 유형: P2PKH, P2SH, P2WPKH, P2WSH, OP_RETURN(데이터 포함, 소각).

## 트랜잭션 전파와 합의
- 트랜잭션은 노드의 메모리풀(mempool)에 들어가며, 이후 블록에 포함되면 확정(confirmed).
- 미확정 상태(transaction confirmations == 0)에서는 되돌리기(체인 리오가 등) 가능성 존재.

## 고급: 부모/자식 관계와 번들
- 자식-부모(ancestor/descendant) 관계는 mempool 정책(수수료, 갯수 제한)에 영향.
- RBF(Replace-By-Fee): 수수료를 올리기 위해 트랜잭션 대체 가능(선택적).

## 참고
- BIP-141, BIP-143 (SegWit 서명 해시), BIP-125 (RBF)
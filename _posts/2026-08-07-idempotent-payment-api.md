---
title: "멱등성 없는 결제 API는 언젠가 터진다"
date: 2026-08-07 19:30:00 +0900
tags: [API Design, Payments, Idempotency]
category: backend
excerpt: "네트워크 재시도로 중복 결제가 났다. Idempotency-Key 설계와 분산 락 대신 유니크 제약으로 푼 과정."
---

결제 요청이 타임아웃 났고, 클라이언트가 재시도했다. 그런데 서버는 첫 요청도 정상 처리한 상태였다. 결과는 **중복 결제**.

## Idempotency-Key

클라이언트가 요청마다 고유한 `Idempotency-Key`를 헤더로 보내고, 서버는 그 키로 **"이미 처리한 요청인지"**를 판단한다. 같은 키가 다시 오면 새로 처리하지 않고 이전 결과를 그대로 돌려준다.

## 분산 락 대신 유니크 제약

처음엔 Redis 분산 락으로 막으려 했지만, 락은 타이밍 이슈와 장애 포인트가 늘어난다. 대신 결제 테이블에 `idempotency_key`를 **유니크 제약**으로 걸었다.

```sql
ALTER TABLE payments
ADD CONSTRAINT uq_idem UNIQUE (idempotency_key);
```

두 번째 요청은 INSERT에서 유니크 위반으로 실패하고, 그 시점에 기존 결제를 조회해 응답한다. DB가 원자성을 보장하니 락보다 단순하고 안전했다.

## 배운 것

**"재시도는 반드시 일어난다"**를 전제로 API를 설계해야 한다. 멱등성은 옵션이 아니라 결제 API의 기본기다.

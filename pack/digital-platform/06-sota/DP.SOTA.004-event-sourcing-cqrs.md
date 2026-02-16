---
id: DP.SOTA.004
name: Event Sourcing + CQRS
type: sota
status: active
summary: "Хранение immutable event log вместо текущего состояния; CQRS разделяет write model и read models — фундаментальный паттерн персистенции для digital twins"
created: 2026-02-16
edition: "2026-02"
trust:
  F: 4
  G: domain
  R: 0.9
related:
  integrates_with: [DP.SOTA.001, DP.SOTA.003]
  enables: [DP.SOTA.002]
tags: [event-sourcing, cqrs, architecture, digital-twin, audit, time-travel, persistence]
---

# Event Sourcing + CQRS

## 1. Определение

**Event Sourcing** — хранение неизменяемой (append-only) последовательности событий вместо текущего состояния. Текущее состояние = replay событий. **CQRS** (Command Query Responsibility Segregation) разделяет write model (append event) и read models (materialized projections).

## 2. Статус SOTA (февраль 2026)

**Established, широко применяется в production.**

- **AWS IoT Device Shadow**: event-sourcing модель для IoT-двойников
- **Axon Framework** (Java/Kotlin): production ES+CQRS framework
- **Amazon Logistics** (AWS re:Invent 2022): ES+CQRS для last-mile delivery

## 3. Архитектура

| Компонент | Роль |
|-----------|------|
| Event Store | Immutable append-only лог (PostgreSQL или EventStore DB) |
| Command Handler | Валидирует команду → добавляет событие |
| Event Handler | Обновляет read model при новом событии |
| Read Model | Материализованное представление: current_state, SLA status, ETA |

## 4. Ценность для digital twin (DP.SOTA.001)

| Свойство | Применение |
|----------|------------|
| **Полный аудит** | SLA-споры: «в 15:47:32 посылка отсканирована как DELIVERED» |
| **Time-travel** | Реконструкция состояния посылки в любой момент прошлого |
| **ML training data** | Event log = датасет для DP.SOTA.002, DP.SOTA.003 |
| **Multiple read models** | Один лог → SLA dashboard + ETA API + аномалии |

## 5. Варианты реализации

| Вариант | Инструмент | Объём |
|---------|------------|-------|
| Простой | PostgreSQL events table | MVP, < 100K событий/день |
| Масштабируемый | Kafka + Redis/PG | > 100K событий/день |

**Минимальная схема:**
```sql
CREATE TABLE parcel_events (
  id          BIGSERIAL PRIMARY KEY,
  parcel_id   UUID NOT NULL,
  event_type  VARCHAR(50) NOT NULL,
  payload     JSONB,
  occurred_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
CREATE INDEX ON parcel_events (parcel_id, occurred_at);
```

## 6. Источники

- Fowler, "Event Sourcing" (martinfowler.com, 2005) — канонический паттерн
- Young, "CQRS Documents" (2010)
- AWS re:Invent 2022: "Building Event-Driven Architectures for Last-Mile Delivery"

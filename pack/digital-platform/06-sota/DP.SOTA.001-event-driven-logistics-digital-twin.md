---
id: DP.SOTA.001
name: Event-Driven Logistics Digital Twin
type: sota
status: active
summary: "Архитектура цифрового двойника физического объекта (посылка) на основе event stream: каждый физический скан — state transition двойника"
created: 2026-02-16
edition: "2026-02"
trust:
  F: 3
  G: project
  R: 0.7
related:
  integrates_with: [DP.SOTA.004]
  enables: [DP.SOTA.002]
tags: [digital-twin, logistics, event-driven, state-machine, last-mile, iot]
---

# Event-Driven Logistics Digital Twin

## 1. Определение

**Event-Driven Logistics Digital Twin** — архитектурный паттерн, при котором каждый физический объект логистики (посылка) имеет виртуальный двойник в программной системе, синхронизированный с физическими событиями через event stream. Каждый скан/событие → state transition двойника → пересчёт SLA-статуса и ETA.

## 2. Статус SOTA (февраль 2026)

**Production-ready, применяется в крупных логистических компаниях.**

- **DHL** (2023): Digital Twins сортировочных центров
- **Maersk** (2023): Digital Twins контейнеров с GPS + sensor fusion
- **Cainiao/Alibaba** (2024): Real-time parcel twin на >100M посылок/день
- **Amazon Logistics**: Event-Driven Architecture для last-mile (AWS re:Invent 2022)

## 3. Архитектура

| Компонент | Роль |
|-----------|------|
| IoT/скан-события | Источники физических событий (barcode scan, GPS) |
| Event broker (Kafka/Pulsar) | Транспорт событий с гарантией доставки |
| State machine (FSM/Statechart) | Модель состояния двойника |
| Event Store | Immutable лог всех событий |
| Projection layer | Read models: SLA status, ETA, risk score |
| API | Внешний доступ к состоянию двойника |

**Типичный FSM посылки:**
```
created → at_warehouse → sorted → in_transit → at_hub
→ at_pvz → out_for_delivery → delivered
                           ↘ delivery_failed → rescheduled
```

## 4. Применение к логистике маркетплейса (ядро A s2r)

- SLA-статус: `current_state + elapsed_time vs SLA threshold`
- Каждая посылка = digital twin, обновляемый по событиям WMS/TMS
- Комбинируется с DP.SOTA.003 (Process Mining) для обнаружения реального FSM
- Персистенция — DP.SOTA.004 (Event Sourcing)

## 5. Ограничения

- При потере событий состояние расходится с реальностью
- При редких сканах (B2B2C) необходим probabilistic state estimation (HMM)

## 6. Источники

- Grigoriev et al., "Digital Twin for Supply Chain and Logistics" (IEEE Access 2023)
- DHL Innovation, "Digital Twins of Sorting Centers" (whitepaper 2023)
- Ivanov & Dolgui, "Digital Supply Chain Twin for Disruption Risk" (IJPR 2021)
- AWS re:Invent 2022: "Building Event-Driven Architectures for Last-Mile Delivery"

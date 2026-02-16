---
id: DP.SOTA.003
name: Process Mining for FSM Discovery
type: sota
status: active
summary: "Автоматическое обнаружение реального жизненного цикла объекта (FSM) из event log через Inductive Miner + conformance checking для выявления аномалий"
created: 2026-02-16
edition: "2026-02"
trust:
  F: 3
  G: project
  R: 0.7
related:
  integrates_with: [DP.SOTA.004]
  enables: [DP.SOTA.001]
tags: [process-mining, fsm, state-machine, event-log, logistics, pm4py, conformance]
---

# Process Mining for FSM Discovery

## 1. Определение

**Process Mining для обнаружения FSM** — применение process mining алгоритмов (Inductive Miner) для автоматического восстановления реального жизненного цикла объекта из event log в виде FSM. Conformance checking выявляет отклонения и аномалии.

**Ключевой принцип:** сначала mine реальный FSM из данных, затем проектировать цифровой двойник — не наоборот.

## 2. Статус SOTA (февраль 2026)

**Established, production-ready.**

- **PM4Py** (Python): активно поддерживаемая библиотека
- **Celonis**: enterprise process mining platform
- **IEEE ICPM 2022**: применение к parcel delivery quality analysis

## 3. IPO-паттерн

| Элемент | Описание |
|---------|----------|
| **Входы** | Event log: (parcel_id, timestamp, scan_type, location_id) |
| **Обработка** | Inductive Miner (PM4Py) → Petri Net → FSM + статистика переходов |
| **Выходы** | FSM с вероятностями переходов, median dwell time per state, аномальные traces |

## 4. Почему запускать первым

При проектировании digital twin (DP.SOTA.001):
1. **Выявляет неожиданные состояния**: «повторная сортировка после ошибки маркировки»
2. **Эмпирические dwell times** → feature set для SLA scorer (DP.SOTA.002)
3. **Conformance checking**: нелегальные переходы → аномалии → SLA risk flags

## 5. Алгоритмы

| Алгоритм | Устойчивость к шуму | Рекомендация |
|----------|---------------------|--------------|
| **Inductive Miner** | Высокая | ✅ Для реальной логистики |
| Alpha Miner | Низкая | Только «чистые» процессы |
| Split Miner | Средняя | Сложные структуры с циклами |

## 6. Источники

- van der Aalst, "Process Mining: Data Science in Action" (Springer 2016)
- Leemans et al., "Inductive Miner" (SIMPDA 2013)
- Berti et al., "PM4Py" (ICPM 2019)
- "Process Mining for Parcel Delivery Quality Analysis" (IEEE ICPM 2022)

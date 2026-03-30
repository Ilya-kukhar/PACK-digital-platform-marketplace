---
id: DP
version: 0.1.0
status: draft
created: 2026-02-16
maintainer: Ilya-kukhar
---

# Pack Manifest: Цифровая платформа

## Идентификатор

- **Pack ID**: `DP`
- **Версия**: 0.1.0
- **Статус**: Draft

## Область

**Название**: Цифровая платформа

**Описание**: Предметная область ИТ-систем, цифровых двойников, ИИ-агентов и архитектурных паттернов для операционных доменов (логистика, биллинг, трекинг и др.). Включает архитектуру цифрового двойника, методы SLA-мониторинга, ИИ-системы и SOTA-паттерны. Переиспользуется в нескольких проектах независимо от конкретного операционного домена.

## Scope

### В scope

- Архитектура цифрового двойника (event-driven, state machine, event sourcing)
- ИИ-системы: Knowledge Extractor, SLA Scorer, маршрутизатор
- SOTA-методы для IT-слоя операционных платформ
- Паттерны хранения и обработки событий (посылки, отправления, транзакции)
- Методы обнаружения FSM из event log (process mining)
- API-контракты между системами
- Failure modes ИТ-систем

### Вне scope

- Бизнес-логика логистики (PACK-marketplace)
- Код конкретных сервисов (downstream-instrument)
- UI/UX (downstream-surface)

## Расширенные виды сущностей

| Код | Вид | Описание | Папка |
|-----|-----|----------|-------|
| `AISYS` | AI System | Паспорт ИИ-системы | `02-domain-entities/` |
| `ARCH` | Architecture | Архитектурное описание | `02-domain-entities/` |

## Entity Index

| ID | Name | Kind | Summary | Status |
|----|------|------|---------|--------|
| DP.SOTA.001 | Event-Driven Logistics Digital Twin | SOTA | Архитектура цифрового двойника посылки на event stream | active |
| DP.SOTA.002 | Calibrated ML SLA Breach Scorer | SOTA | ML-модель с калибровкой для real-time прогноза нарушения SLA | active |
| DP.SOTA.003 | Process Mining for FSM Discovery | SOTA | Обнаружение FSM посылки из event log через Inductive Miner | active |
| DP.SOTA.004 | Event Sourcing + CQRS | SOTA | Персистенция цифрового двойника через immutable event log | active |

## Upstream dependencies

- [Ilya-kukhar/SPF](https://github.com/Ilya-kukhar/SPF) — Second Principles Framework
- FPF — First Principles Framework (через SPF)

## Downstream outputs

- [Ilya-kukhar/s2r](https://github.com/Ilya-kukhar/s2r) — S2R ядро A.Доставленная-Посылка (governance)
- [Ilya-kukhar/DS-extractor-agent](https://github.com/Ilya-kukhar/DS-extractor-agent) — Knowledge Extractor (instrument)

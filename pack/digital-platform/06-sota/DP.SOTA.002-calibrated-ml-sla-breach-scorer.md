---
id: DP.SOTA.002
name: Calibrated ML SLA Breach Scorer
type: sota
status: active
summary: "ML-модель (LightGBM/CatBoost) с калибровкой вероятностей для real-time прогноза нарушения SLA при каждом state transition объекта"
created: 2026-02-16
edition: "2026-02"
trust:
  F: 3
  G: project
  R: 0.7
related:
  integrates_with: [DP.SOTA.001, DP.SOTA.004]
  enables: []
tags: [sla, machine-learning, lightgbm, catboost, calibration, logistics, prediction]
---

# Calibrated ML SLA Breach Scorer

## 1. Определение

**Calibrated ML SLA Breach Scorer** — real-time оценка риска нарушения SLA для каждого объекта в логистической системе. При каждом state transition ML-модель (CatBoost) пересчитывает P(SLA breach). Калибровка (Platt scaling) обеспечивает точность вероятностей. Предупреждение за 12–36 часов до дедлайна.

## 2. Статус SOTA (февраль 2026)

**Production-ready, активно применяется в e-commerce логистике.**

- **Amazon Logistics** (2022–2024): Calibrated ETA prediction
- **Deliveroo/Uber Eats** (2022–2024): Real-time ETA calibration
- **Cainiao/Alibaba**: SLA breach prediction (KDD 2023)

## 3. IPO-паттерн

| Элемент | Описание |
|---------|----------|
| **Входы** | current_state, elapsed_time_hours, remaining_steps, courier_zone_sla_rate, hub_queue_depth, is_weekend, city |
| **Обработка** | CatBoost inference → raw score → Platt scaling → P(breach ∈ [0,1]) |
| **Выходы** | P(SLA breach), severity: low/medium/high, рекомендуемое действие |

## 4. Применение (SLA 1 день / 1-3 дня)

- SLA 1 день: флаг при P > 0.4 на warehouse intake → превентивная приоритизация
- SLA 1-3 дня (B2B2C): пересчёт при каждом межхабовом переходе
- High severity → эскалация диспетчеру / замена курьера / изменение маршрута

## 5. Особенности

- **CatBoost**: обрабатывает categorical features (courier_id, hub_id, city) нативно
- **Калибровка обязательна**: raw probabilities некалиброваны → Platt scaling
- Trigger: интегрируется с event stream DP.SOTA.001

## 6. Источники

- Prokhorenkova et al., "CatBoost" (NeurIPS 2018)
- Kuleshov et al., "Accurate Uncertainties via Calibrated Regression" (ICML 2018)
- Gao et al., "Delivery Time Prediction via Survival Analysis" (KDD 2022 workshop)

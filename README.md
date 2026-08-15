# CCI(9) + DMI(14) + PSAR Backtest — Visual Edition

기존 CCI/DMI/PSAR KOSPI200 백테스트 로직을 유지하면서,
볼린저/Hurst 백테스트처럼 GitHub Actions 실행 후 CSV와 이미지 결과를 한 번에 볼 수 있도록 만든 버전입니다.

## 기존 전략 로직

### Entry
오늘 종가에서 다음 조건을 모두 만족:

1. CCI(9)가 0선을 상향 돌파
2. Close > PSAR
3. +DI(14) > -DI(14)

실제 체결은 다음 거래일 시가.

### Exit
오늘 종가에서 아래 중 하나라도 만족:

1. CCI(9) < 0
2. Close < PSAR
3. +DI(14) <= -DI(14)

실제 체결은 다음 거래일 시가.

기존 전략 자체는 변경하지 않았습니다.

## 기본 Universe

현재 KOSPI200 구성종목을 고정하여 전체 백테스트 기간에 적용합니다.
즉 survivorship bias는 의도적으로 무시합니다.

## 결과 파일

GitHub Actions 실행 후 `output/`에 다음 파일이 생성됩니다.

### 핵심 CSV

- `summary.csv`
  - 포트폴리오 누적수익률
  - CAGR
  - MDD
  - Sharpe
  - 전체 거래 횟수
  - 전체 승률
  - 평균 거래수익률
  - 평균 Buy & Hold 수익률
  - Buy & Hold를 이긴 종목 비율

- `by_stock.csv`
  - 종목별 Strategy Return
  - CAGR
  - MDD
  - Sharpe
  - Win Rate
  - 평균/중앙 거래수익률
  - 거래횟수
  - Buy & Hold
  - 초과수익률

- `trades.csv`
  - 종목
  - 진입일
  - 청산일
  - 진입가격
  - 청산가격
  - 거래수익률
  - 보유기간

- `portfolio_daily.csv`
  - 포트폴리오 일별 Return
  - 누적 Equity

### 추가 CSV

- `top30_strategy_return.csv`
- `top30_trade_count.csv`
- `best50_trades.csv`
- `worst50_trades.csv`

## 자동 생성 이미지

### portfolio_equity.png
동일가중 포트폴리오의 누적 Equity Curve.

### portfolio_drawdown.png
포트폴리오 Drawdown 곡선.

### strategy_vs_buyhold.png
거래횟수가 많은 30개 종목의 전략수익률과 Buy & Hold 수익률 비교.

### trade_return_distribution.png
전체 거래의 수익률 분포.

### top_signal_charts.png
거래횟수가 가장 많은 12개 종목의:

- 종가
- PSAR
- 실제 매수 체결
- 실제 매도 체결

을 한 이미지에서 확인.

## GitHub에서 실행

1. 이 프로젝트 파일을 GitHub 저장소 루트에 업로드
2. GitHub의 `Actions`
3. `CCI DMI PSAR Backtest Visual`
4. `Run workflow`
5. 시작일/종료일/거래비용 입력
6. 완료 후 `Artifacts`
7. `cci-dmi-psar-backtest-results` 다운로드

## 로컬 실행

```bash
pip install -r requirements.txt

python kospi200_cci9_dmi14_psar_backtest_visual.py \
  --online \
  --start 2023-08-10 \
  --end 2026-08-10 \
  --fee-side 0.0 \
  --out output
```

## 참고

이 버전은 기존 CCI 백테스트의 매매조건을 변경하지 않고
결과 분석/시각화 기능만 추가한 버전입니다.

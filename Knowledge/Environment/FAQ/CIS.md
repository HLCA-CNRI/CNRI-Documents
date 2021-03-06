# FAQ - CIS

> CIS 프로젝트 수행 도중 이에 관련하여 알아야 할, 혹은 궁금한 사항 모음

1. CIS가 무엇의 약자인가요? → Carbon Intensity Simulator
   1. 무슨 의미인가요? → 탄소 배출량, 제품 생산량을 통한 탄소집약도 계산
2. 어떻게 탄소 배출량을 계산하나요? → 현재는 배출권거래제 방법론을 통해 산정하며 글로벌 표준인 Greenhouse Gas Protocol로도 업데이트할 예정
3. 탄소 배출량 계산 방식 중 어떤 것을 사용하나요? → 2번과 동일
4. 데이터가 저장이 되나요? → 예, CNRI의 (AWS 기반) 클라우드 데이터베이스에 저장이 됩니다.
   1. 저장된 데이터는 어떻게 사용되나요? → 탄소 배출량 계산 시뮬레이션을 다시 불러올 때 사용됩니다.
5. 시설배치도와 공정도를 업로드하는 이유는 무엇인가요?

   → 보다 정밀한 분석을 위한 도움을 드리기 위해

6. LCI DB가 무엇인가요?

   → LCA 분석에 필요한 데이터베이스, Life Cycle Inventory의 준말

7. Scope 1,2와 Upstream이란게 있는데, 각각이 무엇인지 알 수 있을까요?

   → 직접배출량, 간접배출량 중 전기와 열, 나머지 모든 간접배출량

8. 에너지 종류에 연료/전기/스팀이 있는데, 이외의 에너지 종류에 대해서도 계산 가능한가요?

   → 다른 에너지가 없습니다

9. 생산품별 분석에 할당방식이란게 있는데, 이건 무엇인가요? → 하나의 공장에서 제품이 3가지가 있을 때 하나의 제품이 얼마만큼의 배출량에 기여했느냐를 계산해주기 위한 방법

   1. 각각의 방식은 어떤 차이가 있나요?

      → 대표적인 방식에는 에너지, 질량, 시장가격 기준

10. 2030 예측과 RE100 예측이란게 있는데, 각각은 무엇을 예측하는 것인가요? → 전력 믹스가 두 가지 기준대로 발전했을 때 전력 배출계수

    1. 둘 사이에 어떤 차이가 있나요?
    2. 2030 NDC 상향안이 무엇인가요?

       → NDC는 Nationally Determined Contributions의 준말로 2015년 파리협정 이후 국가별로 특정 기간까지 온실가스 배출량을 얼마만큼 줄이겠다는 목표를 설정하는 것을 말하고, 저희는 우선 전력 믹스 변화로 미래 예측해주는거

    3. RE100 상황의 전력 믹스란 무엇인가요?

       → RE100은 기업들이 사용하는 전력을 모두 신재생에너지로 전환했을 때의 상황

11. 이 제품(Saas)을 사용하기에 적합한 기업(또는 산업 분야)에 어떤 것들이 있을까요?

    → 탄소 배출량이 높은 업계, 혹은 고객의 친환경 니즈가 큰 업계

12. 엑셀 등의 스프레드시트 파일 업로드를 통해 데이터를 넣을 수 있나요?
    1. 향후에 기능 지원할 계획이 있나요? → Y
13. 온실가스 배출권 거래제 명세서 Tier 1이란 무엇인가요?

    → Tier가 3가지가 있고 복잡해서 하나로 딱 잘라 설명하긴 어렵고, Tier가 높아질수록 디테일하게 계산한다 정도만 아셔도 될거 같습니다

14. 청정수소 인증제가 무엇인가요?

    → 수소를 생산하기 위해 배출하는 탄소를 LCA 관점에서 배출량이 특정 기준 이하일 떄는 '청정'수소라고 정의하고 그러한 수소를 판매하는 사업자에게 인센티브 제공하기 위한 제도

## CIS Product - Inner Concepts

- 탄소 배출량 계산
  - CO2 / CH4 / N20 -> 각 기체별 탄소 배출량이 다름
  - <u>Quantity</u> \* Emission Factor
    - 에너지 사용량: 연료 -> 연소
    - 투입량 -> 공정
  - 배출 종류
    - 연소(탄화수소 + 산소 = 물 + CO2)
    - 공정(화학 반응)
    - 탈루(fugitive emission): ex. 천연가스 파이프라인 내부 압력 조절 과정에서의 배출
  - CO2e: CO2 equivalent = CO2 환산 배출량
    - 온실가스를 모두 CO2로 환산한 값
      - cuz CO2가 온실가스의 대부분을 차지하기 때문
- Scope 1, 2, 3

  - Scope 1: 직접배출량(연소, 공정 포함)
  - Scope 2: 간접배출량 중 전기, 스팀
  - Scope 3(Upstream): Scope 1, 2 제외한 나머지

- LCI DB (Scope 3 중 Upstream의 Emission Factor)

  - Upstream Emission Factor가 모여 있는 것
  - CIS에는 Upstream Category 1(Purchased Goods and Services) 하나만 들어가 있음
  - LCI DB는 결국 Upstream Category 1의 Emission Factor라고 할 수 있음

- GWP: Global Warming Potential

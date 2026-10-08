# JU SeqWorkbench Alpha — 상세 기능 및 컨트롤 레퍼런스

버전: v0.2  
대상: JU SeqWorkbench Alpha 0.1.x  
목적: 사용자 가이드의 워크플로 설명을 보완하고, 각 분석/분류 창의 버튼·체크박스·입력 옵션이 실제로 어떤 동작을 하는지 빠르게 확인하기 위한 참조 문서입니다.

> 이 문서는 **사용 방법을 빠르게 찾는 레퍼런스**입니다. 생물학적 결과의 해석이나 연구 결론을 자동으로 보증하지 않습니다. 중요한 결과는 원자료와 함께 별도로 확인하세요.

---

## 1. 문서 사용 방법

- **User Guide**: 처음 사용할 때 전체 워크플로를 익히는 문서
- **이 문서**: 특정 버튼, 체크박스, 옵션의 의미를 확인하는 문서
- UI 언어에 따라 메뉴명은 한국어/영어로 표시될 수 있지만, 내부 동작은 동일합니다.

이 문서는 특히 다음 기능의 세부 옵션을 설명합니다.

1. ID/name Grouping
2. AA Marker Classification
3. Similarity Clustering
4. Point Visualization
5. Region Visualization
6. Sanger AB1 workflow
7. External MSA
8. Single Sequence Editor

---

# 2. ID/name Grouping

ID 또는 description/header 문자열에 포함된 패턴을 이용하여 서열을 그룹으로 분류합니다.

## 2.1 Ruleset Editor

| 컨트롤 | 설명 |
|---|---|
| **Ruleset** | 현재 사용할 규칙셋을 선택합니다. |
| **New** | 새 사용자 규칙셋을 만듭니다. |
| **Save** | 현재 규칙셋과 그룹/패턴을 저장합니다. |
| **Delete (user only)** | 사용자 생성 규칙셋만 삭제합니다. |
| **Copy...** | 현재 규칙셋을 복사하여 새 사용자 규칙셋으로 만듭니다. |
| **Rename...** | 사용자 규칙셋 이름을 변경합니다. |
| **Default group (unmatched)** | 어떤 패턴에도 매칭되지 않은 서열이 들어갈 그룹 이름입니다. 기본값은 `Unclassified`입니다. |
| **+ Group** | 새 그룹 규칙을 추가합니다. |
| **- Delete** | 선택한 그룹 규칙을 삭제합니다. |
| **Copy** | 선택한 그룹 규칙을 복사합니다. |
| **Selected group name** | 현재 선택한 그룹의 이름을 편집합니다. |
| **Patterns** | 해당 그룹에 사용할 패턴을 한 줄에 하나씩 입력합니다. 기본 동작은 ID/header 문자열에 대한 부분문자열 매칭입니다. |

### 패턴 매칭의 기본 개념

분류 대상 문자열은 sequence ID와 description/header를 함께 사용합니다.

기본적으로 패턴은 **부분문자열(substring)** 로 검색합니다. 예를 들어 패턴 `H5N1`은 `sample_H5N1_2024` 안에서 찾을 수 있습니다.

짧은 약어가 너무 넓게 매칭되는 것을 막기 위해 아래의 boundary 옵션을 사용할 수 있습니다.

---

## 2.2 Grouping options

| 옵션 | 기본값 | 설명 |
|---|---:|---|
| **Case-sensitive matching** | ON | 대문자와 소문자를 서로 다른 문자로 취급합니다. OFF이면 대소문자를 구분하지 않습니다. Include/Exclude 필터에도 같은 정책이 적용됩니다. |
| **Show result table** | OFF | 분류 결과 테이블을 표시합니다. |
| **Save grouped FASTA files** | ON | 분류된 그룹별 FASTA를 저장합니다. |
| **Show/save all overlapping rules (multi-match)** | OFF | OFF이면 규칙 순서상 처음 매칭된 그룹 하나를 사용합니다. ON이면 한 서열이 여러 그룹에 동시에 속할 수 있습니다. |
| **Matched display** | `group:pattern` | 결과의 `Matched` 열을 `Group:Pattern` 형식으로 표시할지, `Pattern`만 표시할지 선택합니다. |
| **Remove duplicate Matched values** | ON | multi-match 결과의 Matched 표시에서 중복 문자열을 제거합니다. |
| **Prevent overmatching short abbreviations** | ON | Smart boundary. 1–3자의 짧은 영숫자 코드와 1–4자리 숫자 코드를 독립 토큰처럼 매칭하여 과매칭을 줄입니다. |
| **Match every pattern as a whole token** | OFF | 모든 패턴에 boundary 매칭을 강제합니다. 이 옵션을 켜면 Smart boundary는 비활성화됩니다. |
| **Include** | 비어 있음 | 분류하기 전에 ID/header에 이 부분문자열이 포함된 서열만 남깁니다. |
| **Exclude** | 비어 있음 | 분류하기 전에 ID/header에 이 부분문자열이 포함된 서열을 제외합니다. |

### Smart boundary 예

Smart boundary가 켜져 있고 패턴이 `HA`라면:

- `sample_HA_01` → 매칭
- `xHAy` → 매칭하지 않음

`_`(underscore)는 영숫자가 아니므로 boundary로 취급됩니다.

반대로 **문자열 안에 붙어 있는 패턴까지 모두 찾고 싶다면 boundary 관련 옵션을 모두 꺼야 합니다.**

예를 들어 패턴이 `CPA`이고 ID가 `14CPA`인 경우:

- **Prevent overmatching short abbreviations = ON** → `CPA` 앞의 `4`가 영숫자이므로 독립 토큰으로 보지 않아 매칭하지 않음
- **Match every pattern as a whole token = ON** → 전체 패턴에 boundary를 요구하므로 매칭하지 않음
- **두 옵션 모두 OFF** → 일반 substring 매칭으로 동작하여 `14CPA` 안의 `CPA`를 매칭

따라서 `14CPA`, `XCPA`, `preCPApost`처럼 다른 문자와 붙어 있는 패턴까지 찾으려면 **Smart boundary와 whole-token matching을 모두 해제**하십시오.

### 규칙 순서

Multi-match가 OFF일 때는 **앞쪽 규칙이 우선**합니다. 겹치는 패턴이 있다면 더 구체적인 규칙을 위쪽에 두는 것이 안전합니다.

---

# 3. AA Marker Classification

지정한 AA 위치와 허용 아미노산 조건을 이용해 서열을 그룹으로 분류합니다.

## 3.1 Ruleset Editor

| 컨트롤 | 설명 |
|---|---|
| **Ruleset** | 사용할 AA marker 규칙셋을 선택합니다. |
| **New** | 새 규칙셋을 만듭니다. |
| **Save** | 현재 규칙셋을 저장합니다. |
| **Delete (user only)** | 사용자 규칙셋을 삭제합니다. |
| **Copy...** | 현재 규칙셋을 복사합니다. |
| **Rename...** | 사용자 규칙셋 이름을 변경합니다. |
| **Default group** | 어떤 그룹 규칙에도 맞지 않을 때 사용할 그룹입니다. |
| **+ Group / - Group / Copy** | 그룹 규칙을 추가, 삭제, 복사합니다. |
| **Import rules** | 다른 규칙을 현재 규칙에 가져옵니다. 가져오기 시 현재 규칙을 대체하거나 뒤에 추가할 수 있습니다. |
| **Conditions (AND)** | 한 그룹에 속하기 위해 모두 충족해야 하는 AA 조건들입니다. |
| **Position** | AA 기준 1부터 시작하는 위치입니다. |
| **Allowed AA** | 해당 위치에서 허용할 AA입니다. 여러 문자를 입력하면 그중 하나를 허용합니다. |
| **+ Condition / - Condition** | 선택 그룹의 조건을 추가/삭제합니다. |
| **Selected group name** | 선택 그룹 이름을 편집합니다. |

### Allowed AA 입력 예

다음 입력은 모두 지원됩니다.

- `D`
- `DE`
- `D,E`
- `D E`
- `D|E`
- `*`
- `-X`

쉼표, 공백, 세미콜론, `/`, `|` 등은 입력 구분자로 사용할 수 있습니다. `X`, `*`, `-`도 marker token으로 사용할 수 있습니다.

### 조건 관계

한 그룹 안의 여러 조건은 **AND**입니다. 예를 들어:

- Position 145 = `D`
- Position 156 = `K/R`

이라면 두 조건을 모두 만족해야 해당 그룹에 매칭됩니다.

---

## 3.2 Run options

| 옵션 | 기본값 | 설명 |
|---|---:|---|
| **Filter** | 비어 있음 | ID 또는 header/description에 지정 부분문자열이 포함된 서열만 분류합니다. |
| **Stop codon handling** | UI 선택값 | AA 분석 준비 과정에서 stop codon을 처리하는 방법을 선택합니다. |
| **Truncate at first stop** | 선택 가능 | 첫 stop 이후를 잘라서 사용합니다. |
| **Keep stop symbols** | 선택 가능 | `*`를 유지한 채 분석합니다. |
| **Exclude sequences with stop codons** | 선택 가능 | stop codon이 있는 서열을 분류 대상에서 제외합니다. |
| **Trim NT length to a multiple of 3** | ON | NT 기반 서열을 AA로 준비할 때 길이를 3의 배수에 맞춥니다. |
| **Show/save all matching rules** | OFF | OFF이면 처음 매칭되는 그룹 하나를 사용하고, ON이면 동시에 맞는 모든 그룹에 포함될 수 있습니다. |
| **Run** | — | 현재 화면의 규칙을 저장하지 않았더라도 그 상태 그대로 분류를 실행합니다. |
| **Cancel** | — | 실행하지 않고 창을 닫습니다. |

---

# 4. Similarity Clustering

현재 서열들의 pairwise similarity를 계산한 뒤 지정 임계값 이상을 연결하여 클러스터를 만듭니다.

## 4.1 Comparison settings

| 컨트롤 | 기본값 | 설명 |
|---|---:|---|
| **NT / AA** | NT | 비교 기준을 선택합니다. NT 모드에서는 NT source를, AA 모드에서는 AA 분석 서열을 사용합니다. |
| **Cluster threshold** | 99.0% | 두 서열의 similarity가 이 값 이상일 때 같은 연결 성분(cluster)으로 묶일 수 있습니다. |
| **Full sequence** | ON | 전체 비교 서열 범위를 사용합니다. |
| **Manual range** | OFF | 지정한 1-based inclusive 범위만 비교합니다. 예: `100-500`. 현재 선택한 NT/AA 좌표계를 그대로 사용합니다. |
| **Include gaps (`-`)** | OFF | ON이면 gap 위치도 비교에 포함하며 `-` 대 `-`는 match로 계산합니다. OFF이면 gap이 포함된 위치는 비교에서 제외합니다. |

### NT ambiguity handling

| 옵션 | 설명 |
|---|---|
| **Ignore ambiguous positions** | N/R/Y 등 애매한 IUPAC 염기가 포함된 위치를 비교에서 건너뜁니다. |
| **Compatible IUPAC match** | 가능한 염기 집합이 서로 겹치면 match로 계산합니다. |
| **Strict exact match** | 문자 그대로 동일한 경우만 match로 계산합니다. |

### AA ambiguity handling

| 옵션 | 설명 |
|---|---|
| **Ignore X/*** | X 또는 `*`가 포함된 위치를 비교에서 제외합니다. |
| **Strict exact match** | 문자 그대로 동일할 때만 match입니다. |

---

## 4.2 Result display

| 옵션 | 기본값 | 설명 |
|---|---:|---|
| **Show variation range summary table and highlight on click** | ON | 클러스터 간/내 차이가 나타나는 범위 요약 테이블을 표시하고, 결과 선택 시 관련 위치를 뷰어에서 하이라이트합니다. |
| **Show similarity heatmap** | OFF | similarity matrix를 heatmap으로 표시합니다. |
| **Reorder heatmap by cluster order** | ON (heatmap 사용 시) | heatmap 행/열을 클러스터 순서로 재정렬합니다. Heatmap을 켜야 활성화됩니다. |
| **Show dendrogram (average linkage)** | OFF | similarity 기반 average-linkage dendrogram을 표시합니다. 큰 데이터에서는 제한/경고가 적용될 수 있습니다. |
| **Auto-switch display mode when needed** | ON | 결과 하이라이트가 비교 모드와 맞도록 필요 시 viewer 표시 기준 전환을 제안/처리합니다. 분석 계산 결과 자체를 바꾸는 옵션은 아닙니다. |

## 4.3 Export

| 옵션 | 기본값 | 설명 |
|---|---:|---|
| **Save cluster FASTA files** | OFF | 각 클러스터의 FASTA를 저장합니다. |
| **Export result bundle to a folder** | OFF | 관련 결과 CSV/FASTA 등을 하나의 폴더에 묶어 저장합니다. |
| **Save similarity matrix CSV** | OFF | full similarity matrix를 CSV로 저장합니다. Result bundle을 켰을 때 활성화되며, 큰 데이터에서는 출력 파일이 매우 커질 수 있습니다. |

---

# 5. Point Visualization

알고 있는 특정 위치(site/hotspot)를 선택하여 AA, NT 또는 Codon 단위로 결과를 확인합니다.

## 5.1 Input / analysis controls

| 컨트롤 | 기본값 | 설명 |
|---|---:|---|
| **Preset** | Direct input | 저장한 site preset을 불러옵니다. |
| **Save** | — | 현재 position 설정을 새 preset으로 저장합니다. |
| **Overwrite selected preset** | — | 선택된 사용자 preset을 현재 값으로 덮어씁니다. |
| **Delete** | — | 선택한 사용자 preset을 삭제합니다. |
| **Unit** | 메뉴에서 결정 | AA / NT / Codon 중 어떤 Point Visualization 메뉴에서 열었는지에 따라 고정됩니다. |
| **Positions** | — | 분석할 위치를 입력합니다. AA/NT는 `10,25,50` 같은 1-based 위치 목록을 사용합니다. Codon은 `1-3,15-17` 또는 `[1,2,3],[15,16,17]` 같은 3-nt 그룹을 사용합니다. |
| **Stop codon handling** | AA에서 사용 | AA 준비 과정의 stop 처리 방식을 선택합니다. |
| **Include reference sequence** | OFF | reference 서열도 count/detail 계산 대상에 포함합니다. OFF이면 reference는 비교 기준으로 사용되지만 sample count에서는 제외됩니다. |
| **Create per-strain detail table** | OFF | 각 strain과 각 요청 위치별 세부 결과를 생성합니다. 큰 요청에서는 처리량과 결과 행 수가 증가합니다. |
| **Trim length to a multiple of 3** | ON (AA) | NT→AA 준비 시 3의 배수 길이로 맞춥니다. NT/Codon Point 창에서는 표시되지 않습니다. |
| **Filter** | 비어 있음 | ID 또는 description에 해당 부분문자열이 포함된 서열만 사용합니다. 현재 동작은 단순 부분문자열이며 대소문자를 구분합니다. |
| **Reference** | Automatic selection | 분석 기준 reference를 지정합니다. 자동 선택은 현재 선택 상태와 사용 가능한 서열을 기준으로 reference를 정합니다. |
| **Convert viewer display to the analysis basis** | OFF | 분석 결과와 viewer 하이라이트 연결을 위해 필요할 때 viewer 표시 기준을 분석 기준에 맞춥니다. 기본 OFF입니다. |
| **Run / Run again** | — | 분석을 실행합니다. |

> `Include reference sequence`는 **현재 분석 sample에 reference를 포함할지**를 정하는 옵션입니다. 향후 고려 중인 “그림 export에 reference track 자체를 붙이는 옵션”과는 다른 개념입니다.

---

## 5.2 Visualization tab

사용 가능한 visualization은 모드와 데이터에 따라 달라질 수 있습니다.

- Logo plot
- Codon logo plot
- Heatmap (Position × residue/token)
- Binary mutation map
- Categorical mutation map
- Entropy
- Major allele frequency
- Excluded count
- Bar (total variants)
- Stacked bar (composition)

| 컨트롤 | 기본값 | 설명 |
|---|---:|---|
| **Bits (information)** | ON | Logo 계열에서 정보량(bits) 기반 표시를 사용합니다. |
| **Include `-`** | ON | gap을 visualization 계산/표시에 포함합니다. |
| **Include `*`** | ON | stop token을 포함합니다. |
| **Include `X`** | ON | unknown AA를 포함합니다. |
| **Title / X label / Y label / X ticks / Y ticks** | ON | 그림 요소를 각각 표시/숨깁니다. |
| **Scale** | 100% | 그림 크기를 조정합니다. |
| **Text** | 10 pt | figure text size를 조정합니다. |
| **Font** | 시스템에 따라 선택 | Arial, Times New Roman, DejaVu Sans 및 설치된 일부 한글 폰트를 선택할 수 있습니다. |
| **Style: Default** | 기본 | 일반 화면/출력 스타일입니다. |
| **Style: Publication** | 선택 가능 | 더 큰 figure, 읽기 쉬운 label, white background, 600-DPI raster export 기준을 사용합니다. |
| **Render again** | — | 분석 결과를 다시 계산하지 않고 현재 visualization/style 설정으로 다시 그릴 때 사용합니다. |
| **Save figure...** | — | 현재 figure를 파일로 저장합니다. |

## 5.3 Counts / Detail tabs

| 컨트롤 | 설명 |
|---|---|
| **Filter** | 결과 테이블의 여러 열을 대상으로 표시 행을 필터링합니다. |
| **Sort: Input / Ascending / Descending** | 입력 순서 또는 오름/내림차순으로 정렬합니다. |
| **Export Counts CSV** | Counts 결과를 CSV로 저장합니다. |
| **Detail view: Individual strains** | strain별 개별 행으로 표시합니다. |
| **Detail view: Group by variant** | 같은 variant를 묶고 ID들을 한 셀에 표시합니다. |
| **Export Detail CSV** | Detail 결과를 CSV로 저장합니다. |

---

# 6. Region Visualization

하나 이상의 연속 구간을 AA / NT / Codon 단위로 분석하고 시각화합니다.

## 6.1 Basic controls

| 컨트롤 | 설명 |
|---|---|
| **Preset / Save / Overwrite / Delete** | Region 설정을 preset으로 불러오거나 저장/수정/삭제합니다. |
| **Unit** | `AA`, `NT`, `Codon` 중 분석 단위를 선택합니다. |
| **Regions** | 하나 이상의 구간을 입력합니다. 예: `10-50,120-180`, `10-50;120-180`, 또는 `HA1:10-50,HA2:60-120`. |
| **Layout: Panel** | 각 region을 별도 panel로 표시합니다. |
| **Layout: Concatenate** | 여러 region을 연결한 하나의 연속 축처럼 표시합니다. |
| **Layout: Summary** | region별 요약 결과 중심으로 표시합니다. |
| **Reference** | reference 기반 metric에서 사용할 기준 서열을 선택합니다. Entropy처럼 reference가 필요 없는 metric에는 기준 자체가 계산식에 사용되지 않습니다. |

## 6.2 Metrics

| Metric | 의미 |
|---|---|
| **Entropy** | 각 위치/코돈의 token 다양성을 측정합니다. reference 대비 값이 아닙니다. |
| **Valid sample size** | 해당 위치에서 계산 가능한 유효 sample 수를 표시합니다. |
| **Excluded count** | 해당 위치에서 계산에서 제외된 sample 수를 표시합니다. |
| **Gap rate** | gap 비율을 표시합니다. |
| **Unknown rate** | unknown token 비율을 표시합니다. AA에서는 X, NT/Codon에서는 N 계열 unknown 처리에 사용됩니다. |
| **Non-ref burden** | 선택 reference와 다른 token의 부담/차이를 region 단위로 요약합니다. |
| **Mutation map (binary)** | reference와 같음/다름을 strain × position 형태로 보여주는 binary mutation map입니다. |
| **Strain-wise Region Profile** | strain별 region 차이 패턴을 profile 형태로 표시합니다. |

## 6.3 Strain-wise Region Profile controls

| 컨트롤 | 기본값 | 설명 |
|---|---:|---|
| **Binary non-ref** | 선택 가능 | 각 위치가 reference와 다른지 여부를 strain별 profile로 표시합니다. |
| **Local non-ref density** | 선택 가능 | 지정 window 안에서 non-reference 비율을 이동 평균 형태로 계산합니다. |
| **Burden summary** | 선택 가능 | strain별 전체 burden을 요약합니다. |
| **Group mean profile** | Alpha에서 비활성 | 향후 명시적 grouping metadata와 연결할 용도로 예약되어 있으며 Alpha에서는 사용할 수 없습니다. |
| **Average range (positions)** | 20 | Local non-ref density의 smoothing window입니다. 작을수록 변화가 날카롭고, 클수록 부드럽게 평균화됩니다. |
| **Fix y-axis to 0-1** | OFF | Local non-ref density의 y축을 0–1로 고정합니다. OFF이면 자동 scale을 사용합니다. |
| **Mean line / Median line / Off** | Mean line | 개별 density profile 위에 그릴 summary line을 선택합니다. |
| **Top 20 / Top 50 / All** | Top 50 | Burden summary에서 표시할 strain 수를 선택합니다. |
| **Group source** | 비활성 | Alpha에서는 grouping metadata source를 아직 제공하지 않습니다. |

## 6.4 Variable-position labels

| 컨트롤 | 기본값 | 설명 |
|---|---:|---|
| **Change label threshold** | 0.5 | Entropy가 이 값 이상인 위치/chunk를 추가 label 후보로 사용합니다. |
| **Show variable position labels** | ON | 조건을 만족한 변화 위치의 추가 X-axis label을 표시합니다. |
| **Variable label mode: Clustered** | 기본 | label이 너무 조밀하지 않도록 묶어서 표시합니다. |
| **Top N** | 선택 가능 | 변화가 큰 위치 일부만 표시합니다. |
| **Threshold** | 선택 가능 | threshold 이상인 위치를 기준으로 표시합니다. |
| **All (debug)** | 선택 가능 | 가능한 label을 모두 표시하는 진단용 선택지입니다. |
| **Off** | 선택 가능 | 추가 variable-position text label을 끕니다. |
| **Color X-axis annotation labels by region** | OFF | region별 muted color로 position/annotation label을 구분합니다. |

## 6.5 Figure options

| 컨트롤 | 기본값 | 설명 |
|---|---:|---|
| **Include gap / unknown / stop** | metric에 따라 적용 | 해당 token을 계산에 포함할지 정합니다. 사용하지 않는 metric에서는 비활성화될 수 있습니다. |
| **Title / X label / Y label / X ticks / Y ticks** | ON | 그림의 해당 요소를 표시/숨깁니다. |
| **Scale** | 100% | figure 크기를 조절합니다. |
| **Font size / Font** | 10 pt / 환경별 | figure 글자 크기와 글꼴을 설정합니다. |
| **Style: Default / Publication** | Default | 일반 스타일 또는 publication 스타일을 선택합니다. |
| **Show metrics table** | 분석 후 활성 | 계산된 metric 결과를 표로 엽니다. |
| **Run / Re-render** | — | 분석하거나, 조건이 같으면 가능한 범위에서 기존 계산 결과를 이용해 다시 그립니다. |
| **Save figure** | — | PNG/JPEG 또는 SVG/PDF 등 지원 형식으로 figure를 저장합니다. |

---

# 7. Sanger AB1 Workflow

## 7.1 Import options

| 컨트롤 | 설명 |
|---|---|
| **Show trace window** | Import 후 chromatogram trace 창을 함께 표시합니다. |
| **Import orientation: Original orientation** | basecalled sequence를 원래 방향 그대로 가져옵니다. |
| **Import orientation: Reverse complement** | reverse-complement 방향으로 가져옵니다. |

## 7.2 Trace viewer

| 컨트롤 | 설명 |
|---|---|
| **Orientation** | 표시 방향을 Original / Reverse complement 사이에서 전환합니다. |
| **Previous / Next** | 표시 구간을 이전/다음 base 영역으로 이동합니다. |
| **Base position** | 현재 보고 있는 base 범위를 표시/이동하는 데 사용합니다. |
| **Zoom in / Zoom out / Reset zoom** | chromatogram 확대/축소/초기화입니다. |
| **Trim start / Trim end** | 사용할 basecall 구간의 시작과 끝을 지정합니다. |
| **Import original** | 현재 read를 원본 방향으로 viewer에 가져옵니다. |
| **Import reverse complement** | reverse-complement 방향으로 가져옵니다. |

`Open AB1`은 AB1을 새 viewer/trace workflow로 여는 동작이고, `Import`는 현재 viewer에 sequence를 추가하는 동작입니다.

---

# 8. External MSA

JU SeqWorkbench Alpha는 외부 정렬 바이너리를 포함하거나 자동 다운로드하지 않습니다. **현재 Alpha에서 사용 가능한 정렬 엔진은 사용자가 별도로 설치한 MAFFT입니다.** Clustal Omega 관련 설정/runner 코드는 남아 있지만 Windows Alpha 워크플로가 검증되지 않아 UI 컨트롤이 비활성 상태입니다.

## 8.1 Configure external aligners

| 컨트롤 | 설명 |
|---|---|
| **MAFFT path** | `mafft.bat` 또는 실행 가능한 MAFFT 경로를 지정합니다. |
| **Choose mafft.bat...** | 파일 선택창에서 MAFFT 실행 파일을 지정합니다. |
| **Auto-detect MAFFT** | 저장된 경로/PATH 등에서 MAFFT를 탐색합니다. |
| **Test MAFFT** | 작은 임시 FASTA를 사용해 실제 실행 가능 여부를 확인합니다. |
| **Open MAFFT download page** | 공식 MAFFT 다운로드 페이지를 엽니다. |
| **Clustal Omega 관련 컨트롤** | 현재 Alpha에서는 비활성 상태입니다. 별도 설치 여부와 관계없이 이 릴리스의 지원 실행 경로로 사용하지 않습니다. |
| **Clear paths** | 저장 후보 경로를 지웁니다. Save를 눌러야 변경이 저장됩니다. |
| **Save / Cancel** | 경로 설정을 저장하거나 취소합니다. |

## 8.2 Run external MSA

| 컨트롤 | 설명 |
|---|---|
| **Aligner: MAFFT** | 현재 Alpha에서 지원되는 external aligner입니다. |
| **Clustal Omega** | 현재 Alpha UI에서는 비활성 상태입니다. |
| **Run** | 현재 viewer 서열을 임시 FASTA로 전달하여 MAFFT를 실행합니다. |
| **Cancel** | 실행하지 않습니다. |

## 8.3 MSA completed

| 컨트롤 | 설명 |
|---|---|
| **Open aligned result in new viewer/window** | 현재 viewer를 유지하고 정렬 결과를 새 창에서 엽니다. |
| **Replace current viewer** | 현재 viewer의 working set을 정렬 결과로 바꿉니다. |
| **Also save aligned FASTA** | 위 동작과 함께 aligned FASTA도 저장합니다. |
| **Save now...** | 결과를 즉시 FASTA로 저장합니다. |
| **Show log** | 외부 프로그램의 실행 로그와 진단 정보를 확인합니다. |
| **Cancel** | 결과를 viewer에 적용하지 않고 닫습니다. |

---

# 9. Single Sequence Editor

한 개 sequence를 줄 단위로 보기 쉽게 편집하기 위한 독립 편집 창입니다.

| 컨트롤 | 기본값 | 설명 |
|---|---:|---|
| **ID** | 현재 ID | sequence ID를 편집합니다. Apply 시 중복/유효성 검사가 수행됩니다. |
| **Kind** | 현재 kind | NT/AA 등 현재 sequence 종류를 표시합니다. |
| **Length** | 현재 길이 | 공백/표시용 position label을 제외한 실제 sequence 길이입니다. |
| **Font** | 11 | 편집창 글자 크기를 조절합니다. |
| **Line width** | 80 | 한 줄에 표시할 residue 수입니다. 15–120 범위입니다. |
| **Group: None / 3 / 5 / 10** | 10 | 한 줄 안에서 residue를 몇 개씩 공백으로 묶어 보여줄지 선택합니다. 표시용 공백은 실제 sequence에 저장되지 않습니다. |
| **Overwrite** | OFF | 커서 위치에서 입력할 때 기존 residue를 덮어쓰는 OVR 방식으로 동작합니다. OFF이면 삽입 방식입니다. |
| **Apply** | — | 변경 내용을 parent viewer에 적용합니다. sequence 변경은 지원되는 undo/redo 경로를 사용합니다. |
| **Apply and Close** | — | 적용 후 편집창을 닫습니다. |
| **Close** | — | 적용하지 않고 닫습니다. |

입력 가능한 문자는 현재 NT/AA kind에 따라 검증됩니다. 표시용 line number와 공백은 저장 시 실제 sequence에서 제거됩니다.

---

# 10. 자주 혼동되는 항목

### Reference를 선택하면 reference가 sample count에 자동 포함되나요?
항상 그렇지는 않습니다. Point Visualization에서는 **Include reference sequence** 옵션으로 reference를 sample count/detail에 포함할지 별도로 정합니다.

### ID grouping에서 `_`는 token 구분자로 취급되나요?
Boundary matching에서는 `_`가 영숫자가 아니므로 경계 역할을 합니다.

### Multi-match를 끄면 어떤 그룹이 선택되나요?
규칙 순서에서 **처음 매칭된 그룹**이 선택됩니다. 겹치는 규칙이 있다면 순서가 결과에 영향을 줄 수 있습니다.

### AA Marker 위치는 0부터 시작하나요?
아닙니다. 사용자 입력 위치는 **1-based**입니다.

### Similarity의 Manual range는 NT 좌표를 자동으로 AA 좌표로 바꾸나요?
아닙니다. 현재 선택한 비교 모드(NT 또는 AA)의 좌표를 그대로 사용합니다.

### MSA 프로그램이 JU SeqWorkbench에 포함되어 있나요?
아닙니다. 외부 정렬 바이너리는 포함되지 않습니다. 현재 Alpha에서는 사용자가 별도로 설치한 **MAFFT**를 연결해 사용하며, Clustal Omega 경로는 비활성 상태입니다.

---

# 11. 관련 문서

- [JU SeqWorkbench Alpha 사용자 가이드 — 한국어](user_guide_ko.md)
- [빠른 시작 PDF](JU_SeqWorkbench_Quick_Start_KO.pdf)
- [Alpha Limitations & Cautions](../limitations/index.md)

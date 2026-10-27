# Analysis Methods & Calculation Definitions | 분석 방법과 계산식

**JU SeqWorkbench — public methods documentation (not application source code)**

[English](#english) · [한국어](#한국어)

This page explains selected **definitions, equations, options, and interpretation limits** of JU SeqWorkbench's sequence-analysis workflows. Documenting an algorithm does **not** publish or license the proprietary application source code.

**Verification scope:** The definitions below were checked against an available development-code snapshot dated **2026-10-08** (commit `a23a3c7`). They should not be treated as an exhaustive, independently validated specification of the **Alpha 0.1.1** binary released on 2026-10-25. Release-to-source parity, all presets, and numerical regression tests still require a version-specific audit. This page will be revised as verification proceeds.

---

<a id="english"></a>
## English

### 1. Inputs, coordinates, and denominators

- Analyses operate on the selected **working sequence/alignment state**; they do not perform primary NGS variant calling or de novo alignment.
- Positions are **1-based**, interpreted in the selected NT, amino-acid (AA), or codon-coordinate context. A codon observation comprises three consecutive NT positions.
- The effective sample size **can vary by position**, depending on sequence length, reference inclusion, token filters, and options. Always interpret a percentage together with its denominator.
- **NT** means *nucleotide* for analytical units, covering DNA/RNA representations. The separate Viewer **DNA/RNA/AA** display labels are a UI terminology matter, not a change to NT-level mathematics.

### 2. Point Visualization: residue/codon counts

For a selected alignment position `j`, reference token `r_j`, and eligible sample tokens `x_{i,j}`:

```text
Count_j(a) = Σ_i 1[x_(i,j) = a]
N_j        = number of eligible observations at position j
Frequency_j(a) = Count_j(a) / N_j          (when N_j > 0)
Reference-match count = N_j − Σ_(a ≠ r_j) Count_j(a)
```

Here `1[condition]` equals 1 when true and 0 otherwise. The reference sequence may be included or excluded according to the analysis option. Substitutions are **direct differences from the selected reference token**, not inferred evolutionary events or variant calls from read mapping. The optional per-sequence result identifies which sequence IDs contributed a difference. Codon Point compares **three-base strings**; different codons are counted as different tokens even if they encode the same amino acid.

### 3. Region Visualization: Shannon entropy

At alignment position `j`, calculate token frequency `p_j(a) = n_j(a) / N_j` after applying the selected inclusion/exclusion filters:

```text
H_j = − Σ_(a with p_j(a)>0) p_j(a) log2(p_j(a))      [bits]
Region mean entropy = arithmetic mean of finite H_j values
```

**Example:** Four eligible observations `A, A, G, G` yield frequencies 0.5/0.5 and entropy **1 bit**.

Gap (`-`), unknown (`N`, `X`, `?` where applicable), and stop (`*`) handling depends on the unit and selected options; excluded observations do **not** contribute to the entropy denominator. When no eligible tokens remain, the position-level Region calculation yields **NaN / no valid entropy**, rather than treating it as biologically invariant.

### 4. Region quality and reference-difference rates

The reviewed Region implementation distinguishes **available observed tokens** from **usable tokens** and from **reference-comparable non-reference tokens**:

```text
Valid rate      = usable token count / available token count
Gap rate        = observed gap-token count / available token count
Unknown rate    = observed unknown-token count / available token count
Non-reference rate = non-reference mismatches / comparable non-reference tokens
```

Rates use 0 when their respective denominator is zero in the reviewed implementation; check the accompanying counts to distinguish zero observed events from zero available comparisons. The reference-difference rate requires an eligible reference token and excludes the reference sequence itself. A gap/unknown rate is **not** a sequencing-error estimate.

### 5. Aligned pairwise similarity and clustering

For aligned sequences `A` and `B`, compare eligible columns under the chosen gap/ambiguity policy:

```text
Identity(A,B) [%] = 100 × matched comparable columns / comparable columns
```

- If either sequence contains a gap at a column, that column is skipped when **Include gap** is off.
- NT ambiguity options include **Ignore** (skip ambiguous comparisons), **Compatible** (IUPAC symbol sets with non-empty intersection count as matches), and **Strict** (literal matching under the implementation's supported symbols). AA options include **Ignore** for `X`/`*` and **Strict**.
- In the reviewed implementation, only the shared aligned prefix is compared for unequal lengths; **zero comparable columns return 0%**, not a meaningful estimate of biological divergence.

**Clustering uses thresholded connectivity (single-linkage-style connected components):** create an edge between sequences `i` and `j` when `Identity(i,j) ≥ T`; each connected component becomes a group.

**Interpretation warning:** `A–B ≥ T` and `B–C ≥ T` can put A, B, and C in one cluster even if `A–C < T`. Therefore, membership **does not imply all pairwise identities within a cluster exceed T**. This is similarity-based grouping, not phylogenetic inference.

### 6. Sequence logos: frequency and information bits

The reviewed Point-logo calculation supports raw frequency heights, or information-scaled heights:

```text
H_j     = − Σ_a p_j(a) log2(p_j(a))
I_j     = max(0, log2(K) − H_j)
Height_j(a) = p_j(a) × I_j
```

`K` is set to **4** for NT, **20** for AA, or **64** for codon logos in the reviewed calculation. The information-bits mode shown here does **not** include a small-sample correction term. Token filtering and effective sample counts must be checked before comparing logo heights between datasets. These are **visualization scores**, not statistical significance tests.

### 7. Rule-based grouping (non-statistical)

- **AA Marker Classification** evaluates user-defined residue conditions at specified positions. Depending on the mode, it may assign the first matching group, all matching groups, or the most-specific matching rule. These are explicit user rules, **not trained classifiers**.
- **ID/name grouping** matches configured patterns against sequence identifiers/descriptions, with optional boundary and case rules. It does **not** imply biological relatedness.

### What this page does not claim

This documentation does not establish clinical validation, statistical significance, uncertainty intervals, phylogenetic support, full reproducibility of all historical Alpha builds, or independent verification of every exported graphic. MAFFT is an **external**, separately installed alignment engine; JU SeqWorkbench does not claim authorship of its alignment algorithm.

For practical use, see the [English User Guide](../user-guide/user_guide_en.md) and [Alpha limitations](../limitations/). For method-related questions or reproducible discrepancies, use [Discussions](https://github.com/user92-11/JU-works/discussions) or [Issues](https://github.com/user92-11/JU-works/issues) with synthetic or non-confidential examples.

---

<a id="한국어"></a>
## 한국어

### 1. 입력, 좌표 및 분모

- 분석은 선택한 **현재 작업 서열/정렬 상태**를 대상으로 합니다. NGS read mapping이나 새로운 variant calling을 수행한다는 의미가 아닙니다.
- 위치는 **1부터 시작하는 좌표(1-based)**이며 선택한 NT, AA, Codon 분석 단위에 따릅니다. Codon 단위는 연속된 염기 3개를 하나의 토큰으로 취급합니다.
- 실제 유효 표본 수는 위치별 서열 길이, 참조 포함 여부, gap·unknown 필터 등에 따라 달라질 수 있습니다. 비율은 반드시 **분모와 함께** 해석해야 합니다.
- **NT**는 분석 단위인 뉴클레오타이드를 뜻합니다. Viewer의 DNA/RNA/AA 표시 명칭 문제와는 별개입니다.

### 2. Point Visualization: 염기·아미노산·코돈 빈도

선택 위치 `j`의 참조 토큰을 `r_j`, 비교할 수 있는 서열의 토큰을 `x_(i,j)`라고 하면:

```text
Count_j(a) = Σ_i 1[x_(i,j) = a]
N_j        = 해당 위치의 분석 대상 유효 관측 수
Frequency_j(a) = Count_j(a) / N_j          (N_j > 0)
참조 일치 수 = N_j − Σ_(a ≠ r_j) Count_j(a)
```

`1[조건]`은 조건이 참이면 1, 아니면 0입니다. 참조 서열 포함 여부는 옵션에 따릅니다. 여기서 변이는 선택 참조와의 **문자열 차이**이며 계통학적 치환 사건이나 NGS variant call을 의미하지 않습니다. 결과의 서열별 상세 표는 차이를 보인 ID를 추적하는 용도입니다. Codon Point는 코돈 **문자열 3개 염기**를 비교하므로 동의치환 코돈도 다른 토큰으로 집계됩니다.

### 3. Region Visualization: Shannon entropy

포함 옵션을 적용한 후, 해당 위치에서 각 토큰의 빈도를 `p_j(a) = n_j(a) / N_j`로 계산합니다.

```text
H_j = − Σ_(p_j(a)>0) p_j(a) log2(p_j(a))             [bits]
구간 평균 엔트로피 = 유한한 H_j 값들의 산술평균
```

예를 들어 유효 관측이 `A, A, G, G`라면 각 빈도는 0.5이고 엔트로피는 **1 bit**입니다.

Gap(`-`), unknown(`N`, `X`, `?` 등), stop(`*`)은 분석 단위와 옵션에 따라 포함하거나 제외합니다. 제외한 토큰은 엔트로피의 분모에 들어가지 않습니다. 유효 토큰이 하나도 없으면 해당 위치의 Region entropy는 **NaN(계산 가능한 값 없음)**으로 처리되며, 보존도가 높아 엔트로피가 0이라는 뜻이 아닙니다.

### 4. Region의 표본 및 참조 차이 비율

확인된 Region 구현은 원시 관측 수, 필터 통과 수, 참조와 비교 가능한 비참조 토큰 수를 구분합니다.

```text
Valid rate      = 필터 통과 토큰 수 / 관측 가능한 토큰 수
Gap rate        = gap으로 분류된 토큰 수 / 관측 가능한 토큰 수
Unknown rate    = unknown으로 분류된 토큰 수 / 관측 가능한 토큰 수
Non-reference rate = 참조와 다른 비참조 토큰 수 / 참조 비교 가능 비참조 토큰 수
```

확인한 구현에서는 각 분모가 0일 때 비율을 0으로 반환하므로, 반드시 **관련 count 열을 함께 확인**해야 합니다. 참조 차이 비율은 유효한 참조 토큰이 있어야 계산되며 참조 서열 자체는 비교 대상에서 제외됩니다. Gap이나 unknown 비율이 곧 염기서열 분석 오류율은 아닙니다.

### 5. 정렬 서열 유사도와 군집화

선택된 gap·모호문자 정책을 적용하여 비교 가능한 정렬 열에서의 일치도를 계산합니다.

```text
Identity(A,B) [%] = 100 × 일치한 비교 가능 열 수 / 비교 가능 열 수
```

- **Include gap**을 끄면 어느 쪽이든 gap이 있는 열을 제외합니다.
- NT 모호문자는 **Ignore**(제외), **Compatible**(IUPAC 기호가 뜻하는 염기 집합의 교집합이 있으면 일치), **Strict**(구현상 지원 문자 기준 정확한 문자 일치) 옵션으로 처리합니다. AA에서는 `X`·`*` 제외 또는 엄격 비교 옵션을 제공합니다.
- 길이가 다르면 확인된 구현에서는 두 서열의 **공통 길이 구간만** 비교합니다. 비교 가능한 열이 하나도 없을 때는 **0%**를 반환하지만, 이것을 생물학적 거리의 근거로 해석해서는 안 됩니다.

군집은 임계값 `T`에 대해 `Identity(i,j) ≥ T`인 서열 쌍을 연결한 뒤, **연결 성분(connected component)**을 한 그룹으로 구성합니다. 이는 single-linkage 방식과 유사합니다.

**해석 시 주의:** A–B와 B–C가 임계값을 넘으면 A·B·C가 한 그룹에 속할 수 있지만, A–C는 임계값보다 낮을 수 있습니다. **같은 그룹에 속한다는 사실만으로 모든 서열 쌍이 임계값 이상이라는 뜻은 아닙니다.** 이는 계통수 추론이 아닌 유사도 기반 분류입니다.

### 6. Sequence logo: 빈도 및 정보량(bits)

Point의 Logo plot은 상대빈도 표시 또는 정보량을 반영한 높이 계산을 사용합니다.

```text
H_j          = − Σ_a p_j(a) log2(p_j(a))
I_j          = max(0, log2(K) − H_j)
Height_j(a)  = p_j(a) × I_j
```

확인한 코드에서 `K`는 NT **4**, AA **20**, Codon **64**입니다. 이 정보량 계산에는 작은 표본 수에 대한 보정항(small-sample correction)이 포함되어 있지 않습니다. 그래프 높이를 비교하기 전에 토큰 필터와 유효 표본 수를 확인해야 합니다. **이 값은 시각화 지표이지 통계적 유의확률이 아닙니다.**

### 7. 규칙 기반 그룹 분류

- **AA Marker Classification:** 사용자가 지정한 위치와 허용 아미노산 조건을 대조합니다. 옵션에 따라 첫 일치, 모든 일치, 조건이 가장 구체적인 일치 규칙을 선택합니다. 학습된 AI 분류 모델이 아닙니다.
- **ID/name grouping:** ID/설명 문자열에 사용자 지정 패턴이 있는지 비교하며 대소문자와 경계 규칙을 선택할 수 있습니다. 이 결과만으로 생물학적 유연관계를 주장할 수는 없습니다.

### 범위와 검증 수준

이 페이지는 임상·진단 검증, 통계적 유의성, 신뢰구간, 계통학적 지지도, 모든 Alpha 빌드에 대한 수치 일치 검증을 의미하지 않습니다. MAFFT는 별도 설치하는 **외부 정렬 도구**이며, 그 알고리즘은 JU SeqWorkbench의 자체 개발 알고리즘으로 취급하지 않습니다.

[한국어 사용자 가이드](../user-guide/user_guide_ko.md) · [Alpha 한계 및 주의사항](../limitations/) · [Discussions](https://github.com/user92-11/JU-works/discussions) · [Issues](https://github.com/user92-11/JU-works/issues)

---

**Transparency principle / 공개 원칙:** 공개 대상은 계산의 의미, 수식, 입력·필터 기준, 출력 해석, 알려진 한계입니다. 비공개 애플리케이션 소스코드·내부 구현 파일을 공개한다는 뜻은 아닙니다.

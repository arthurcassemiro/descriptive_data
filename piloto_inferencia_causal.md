# Pilot: is there a causal reading of financing channel on contract and infrastructure quality?

Date: 21 October 2026. Script: `codigo/47_piloto_causal.py`. Tables: `dados/p01_pilot_contract_channel.csv`, `p01b_pilot_balance_credit_vs_not.csv`, `p02_pilot_asset_concession.csv`, `p03_pilot_event_study_deaths.csv`, `p04_pilot_within_contract_tranche.csv`. Estimation with numpy only (OLS, fixed effects by within-transformation, cluster-robust standard errors); no library was available for modern staggered-adoption estimators, so the asset-level results are two-way fixed effects and an event study, with the known caveat.

## 1. What "quality" means here

Two outcomes families, measured on the bases we have.

- Contract quality (state base, 10 states, 2010-2025): extended beyond the original term (yes/no; 12 months or more), value above the original, a year without payment, and the payment stage (months from commitment to payment, carry-over).
- Infrastructure quality (federal highways, 486 highway-state assets): CNT condition scores (2010-2018 only) and PRF deaths per 100 km (2007-2025). No condition panel exists for state roads.

The treatment is the financing channel: credit-paid, credit-dominant, multichannel, earmarked fund, Union transfer (contract level); concession on the asset, channel 5 (asset level).

## 2. Pilot designs and what they show

### A. Contract level: channel against outcome, with state-by-start-year fixed effects, size and type of work

| outcome | treatment | raw gap | FE | FE + log value + object | large third only |
|---|---|---|---|---|---|
| extended | credit-paid | +19.3 pp | +14.5 pp (t 2.9) | +6.4 pp (t 1.4; 1.8 with 90 clusters) | -3.2 pp (ns) |
| extended | multichannel | +22.6 pp | +22.5 pp (t 4.1) | +13.4 pp (t 5.3) | +10.5 pp (t 3.5) |
| extended | earmarked fund | -6.7 pp | -16.1 pp | -8.4 pp (t -1.7 with 90 clusters) | -6.2 pp (t -2.4) |
| extended | Union transfer | -2.4 pp | -16.2 pp (t -2.1) | -10.8 pp (t -1.2 to -1.7) | n/a |
| value above original | credit-paid | +5.1 pp | +9.0 pp (t 1.4) | +1.0 pp (ns) | -9.1 pp (ns) |
| value above original | multichannel | +31.0 pp | +27.0 pp (t 5.2) | +17.7 pp (t 6.4 to 8.3) | +15.4 pp (t 4.6) |
| value above original | Union transfer | -4.6 pp | -20.8 pp (t -1.7) | -21.2 pp (t -2.7 to -3.3) | -2.8 pp (ns) |
| payment gap | credit-paid | +1.6 pp | +1.2 pp (ns) | +0.7 pp (ns) | +3.1 pp (ns) |
| payment gap | multichannel | +4.0 pp | +3.8 pp (t 2.4) | +4.1 pp (t 2.5 to 3.5) | +2.7 pp (ns) |

Reading. The raw gap between credit-paid and other contracts is selection: credit-paid contracts are larger (median R$ 4.1 mn against 2.7 mn), more often capacity expansion (33% against 17%) and longer (20 against 13 months, `p01b`). Once the state-year, the size and the type of work are held constant, the credit effect on extension falls to a third and loses significance, and inside the large third it is zero. The multichannel association survives every control (+13 pp extension, +18 pp value growth, +4 pp gap) but the mix is chosen with the project, so this is a robust association, not an effect. The Union-transfer contracts show less value growth under controls (-21 pp), a result worth a design of its own (the convênio fixes the value). Inference: only 5 to 9 state clusters; the standard errors clustered by state are unreliable (one of them collapses), so the table also reports clustering by state-year (90 to 151 clusters).

### B. Within the same contract: the loan tranche against the Treasury tranche at the payment stage

The cleanest comparison the bases allow. In contracts paid by both a loan and the Treasury in the same year, the note paid from the loan is compared with the note paid from the Treasury, with contract-by-year fixed effects: the work, the contractor, the state and the year are the same; only the channel of the note differs.

| state | outcome | loan note vs Treasury note | notes | contracts |
|---|---|---|---|---|
| MG | months from commitment to payment | -0.90 months (t -6.1) | 421 | 113 |
| MG | note inscribed as carry-over | +21 pp (t 5.1) | 421 | 113 |
| MG | paid 3 months or more after commitment | -15 pp (t -4.4) | 421 | 113 |
| SP | carry-over share of the note (paid as RP over paid) | -13 pp (t -5.1) | 696 | 144 |

Reading. Holding the work constant, the loan tranche is paid faster and carries over less than the Treasury counterpart: the channel changes the payment stage. In MG the loan notes are more often inscribed as carry-over (committed close to the disbursement, at year end) yet paid sooner. This is the one result of the pilot that can be defended as an effect of the channel, with the caveat that the agency chooses which notes to delay, and that choice is the mechanism (the Treasury counterpart is the flexible money). It does not reach the work's quality, only the money's timeliness.

### C. Asset level: concession (channel 5) on federal highways, staggered adoption

Two-way fixed effects (asset and year) on the highway-state panel, switchers against never-conceded and not-yet-conceded assets; the 14 assets conceded before 2001 are excluded. 36 assets switch to a concession in 2008-2024, 305 never do.

| outcome | years | concession effect | t | assets | switching |
|---|---|---|---|---|---|
| CNT overall score (1-5) | 2010-2018 | +0.12 | 1.5 | 298 | 25 |
| CNT pavement score | 2010-2018 | -0.08 | -0.7 | 298 | 25 |
| share of km good or excellent | 2010-2018 | +5.7 pp | 2.0 | 325 | 25 |
| deaths per 100 km | 2007-2025 | -1.27 | -2.0 | 257 | 38 |

Event study on deaths per 100 km (years before and after the concession): leads of -5 to -2 are small and of mixed sign (no pre-trend; largest |t| 1.2); the effect appears from the third year after the concession (-2.5, -1.5, -3.9, -5.7 deaths per 100 km in years 3 to 6 and later, t -2.6 to -3.7). The Union's own spending per km enters with the opposite sign to the one a causal reading would want: more federal money per km goes with a worse condition (-0.32 on the CNT score, t -3.0) and more deaths (+2.2, t 2.0), because money goes where the road is bad. That is the reverse causality that any spending-to-quality design has to break.

## 3. The most viable strategies, in order

1. Staggered difference in differences at the asset level, concession against DNIT direct execution, on road safety (deaths per 100 km, 2007-2025) and, where the window allows, on CNT condition (2010-2018). It is the only design in the pilot with a time dimension, a treatment that switches on at known dates, a control group, and no pre-trend in the pilot. What it needs: the treatment at the kilometre, not the highway (the panel has the km inside and outside the concession stretch; the pilot used the highway flag); traffic as a control or a denominator (ANTT and DNIT counts; deaths per vehicle-km, not per km); a modern estimator for staggered adoption (Callaway and Sant'Anna, or Sun and Abraham), because two-way fixed effects with heterogeneous timing can be biased; placebo tests on neighbouring non-conceded highways and on state roads with CNT; and attention to the PRF series break around 2019-2020.
2. Within-contract tranche comparison at the payment stage (design B), extended to every state with the month of the note (MG now; SP from 2022 with the DER note-contract sheet; the federal notes from 2021 with the source on the note). It identifies a channel effect on timeliness holding the work constant. It cannot speak to the work's quality or to the extension, which are contract-level.
3. Event study of payment gaps around a real-cut year within the state (the fiscal shock as the treatment, timing as the identification): the `eventos_corte_uf_ano` and `eventos_contrato` tables already give 157 state-years with a cut and 689 gaps, 35% of them starting in a cut year. It answers "does the fiscal shock interrupt the credit-paid contract less than the Treasury-paid one", which is the federative question, and it is feasible now.
4. Contract-level comparisons of channel against extension, value growth and gap are not causal and will not become so with more controls: the channel is chosen with the project. They stay descriptive, with state-year fixed effects, size and type of work, and they are what the coding needs to hold constant. An instrument for the channel (CAPAG rating thresholds for guaranteed borrowing, debt over RCL cutoffs) is conceptually right but not feasible with 10 states in the contract base and a handful of rating changes.

## 4. Limitations of the data, stated once

- Selection into the channel: credit pays the large, long, capacity-expansion works; the mix is chosen with the project. Observables remove most of the raw gap; what remains is not identified.
- Few clusters: 10 states in the contract base, 5 to 9 with each outcome. Standard errors clustered by state are unreliable; state-year clustering is a stopgap, not a solution.
- Outcome coverage differs by state: extension needs the term (SP, BA, SC, part of MG and RS); rescission only MG and DNIT; stoppage only SP, SC, RJ; a contract without the field is out of that regression's denominator.
- The federal contract layer starts in 2020-2021 for payments; the federal contract outcomes cannot be followed over the fiscal cycle.
- Infrastructure quality exists only for federal highways: CNT 2010-2018 (then it stops in the panel), deaths 2007-2025 without traffic. State roads have CNT for 199 roads in one cross-section and no panel.
- Treatment at the asset level is coarse: the concession covers part of the highway; the pilot treated the whole highway-state as conceded.
- Two-way fixed effects with staggered adoption and heterogeneous effects can be biased; the pilot's event study is the check, not the proof.
- Linear probability models throughout; no logit, no bootstrap; the pilot is a reading of signs, magnitudes and robustness, not final estimates.
- The link between payments and contracts is itself uneven (Chart 13): São Paulo before 2022 at a fifth to a third, the Union before 2021 at zero. Any contract-level design inherits this coverage.

## 5. What the pilot settles

There is one defensible causal reading today, narrow and at the payment stage: within the same contract, the loan tranche pays faster than the Treasury tranche. There is one promising design for infrastructure quality, the staggered concession difference in differences, with a clean pre-trend in the pilot and an effect on deaths that grows after the third year, which needs kilometre-level treatment, traffic and a modern estimator before it is reported. Channel against contract outcomes at the contract level is association, and the descriptive statistics already say what it looks like once size, type of work and state are held constant.

## 6. Exhaustive sweep: every outcome the bases hold, against channel and sphere (21 October, evening)

Script `codigo/48_varredura_causal.py` (338 regressions, `dados/p05_pilot_sweep_all_outcomes.csv`, event studies in `p06`), robustness in `codigo/49_varredura_causal_robustez.py` (`p07`, `p08`). Same rules: fixed effects by within-transformation, cluster-robust standard errors, a contract without the field is out of that denominator, results with 10 clusters or fewer flagged. Stars: * 10%, ** 5%, *** 1%.

### 6.1 State contracts: 16 outcomes, 8 channel treatments, two designs

Design 1: state-by-start-year fixed effects, log original value, type of work; clustered by state-year (90 to 151 clusters). Design 2: exact cells of state-year by object category by value decile (a matching estimator), same clustering. Coefficients in percentage points (binary outcomes), log points or percent; the two designs agree in sign and size almost everywhere, which is the robustness check.

| outcome | multichannel | domestic credit (3) | external credit (4) | earmarked fund | Union transfer (1b, CIDE) | state company (2) |
|---|---|---|---|---|---|---|
| extended (any) | +13 / +12 *** | +12 / +11 *** | +2 / +2 | -8 / -3 | -11 / -1 | n/a |
| extended 12+ months | +12 / +14 *** | +12 / +13 * | -6 / -4 | +3 / +1 | -7 / +5 | n/a |
| log extension months | +0.48 / +0.50 *** | +0.34 / +0.35 ** | -0.09 / -0.06 | -0.10 / -0.02 | -0.25 / +0.09 | n/a |
| value above original | +18 / +15 *** | +9 / +8 ** | -4 / -1 | -8 / -1 | -21 / -19 *** | -5 / -6 |
| value growth, % | +21 / +17 *** | -4 / -3 | -16 / -7 ** | +6 / +4 | -17 / -6 *** | -27 / -13 *** |
| payment gap | +4 / +4 *** | 0 / -1 | +1 / 0 | -1 / -1 | -1 / +1 | -2 / -3 |
| stoppage (SP, SC, RJ) | -2 / -3 ** | -3 / -4 ** | -2 / -7 * | n/a | 0 / -1 | n/a |
| rescinded (MG) | -4 / -6 ** | n/a | -8 / -3 | +7 / +9 | +14 / +17 * | -7 / -8 *** |
| carry-over share of paid | -2 / -2 | -1 / 0 | -3 / -2 *** | -1 / +1 | +2 / +1 | 0 / 0 |
| financial execution, SP, pp | +0.5 / +0.4 | +1.2 / -0.5 | +2.2 / +2.7 *** | n/a | n/a | n/a |
| log duration | +0.35 / +0.29 *** | +0.15 / +0.11 | +0.06 / +0.03 | -0.33 / -0.17 *** | -0.12 / +0.02 | +0.06 / +0.15 |
| ended by 2025 (started by 2022) | -5 / -5 * | -6 / -5 ** | +3 / +3 | +5 / +1 *** | +1 / 0 | +1 / +2 |
| clean finish, strict | -11 / -9 *** | -11 / -10 *** | -5 / -4 | +8 / +1 | +12 / +1 | n/a |
| clean finish, broad | -4 / -4 *** | +1 / +2 | -1 / 0 | 0 / +1 | -3 / -4 | +10 / +10 *** |

Reading, by treatment.

- Multichannel: worse on every delay outcome (extension, value growth, gap, clean finish), better on stoppage and rescission. Robust to size, type, state-year and exact matching. Not causal: the mix is chosen with the project; it is the signature of the large programme work.
- Domestic credit (BNDES, Banco do Brasil programmes): more extension (+12 pp), more value growth (+9 pp), fewer clean finishes (-11 pp), fewer completions by 2025 (-6 pp), fewer stoppages.
- External credit (IDB, World Bank): no extension effect, lower value growth in percent (-16 / -7), lower carry-over (-3 pp ***), higher financial execution in SP (+2.2 to +2.7 pp ***), fewer stoppages. Lender discipline at the payment stage, no effect on the term. The direct comparison external against domestic within the same state-year (`p08`, 7 states with both) finds no significant difference on any outcome: the two credit types differ across states more than within them, so this contrast is not identified.
- Earmarked fund: shorter contracts (-0.33 log), more completions (+5 pp ***), fewer extensions (-8 pp *); the fund pays the short maintenance contract.
- Union transfer (convênio, CIDE): value above the original 20 pp less often (*** in both designs), rescission higher in MG (+14 to +17 pp *). The transfer fixes the value and, when it stops, the contract ends.
- State company: fewer rescissions, lower value growth, more clean finishes (broad).
- Heterogeneity by fiscal position (CAPAG C or D at the start year, 2018-2025): credit-paid contracts in weak states are extended 17 pp more (t 1.8); nothing on gaps or carry-over.

### 6.2 Sphere: federal direct execution against state contracts

Pooled base (12,910 state, 3,274 federal), start-year fixed effects, log value, type of work, clustered by state (27 clusters).

| outcome | all contracts | largest fifth | capacity expansion only |
|---|---|---|---|
| carry-over share of paid | +21 pp (t 7.4) | +16 pp (t 6.9) | +33 pp (t 9.4) |
| log duration | +0.33 (t 3.2) | +0.16 (t 1.3) | +0.30 (t 2.5) |
| extended (any) | -9 pp (t -1.2) | -22 pp (t -5.4) | -8 pp (t -1.1) |
| extended 12+ months | +4 pp (t 0.8) | -2 pp (t -0.3) | +12 pp (t 1.9) |
| value above original | -8 pp (t -0.6) | -10 pp (t -0.8) | -35 pp (t -2.6) |
| value growth, % | -6 (t -1.0) | +7 (t 1.5) | -21 (t -3.2) |

Reading. Holding size, type and start year constant, the Union's contract carries over a fifth to a third more of its payment, lasts longer, and in capacity expansion grows less in value; among the largest contracts it is extended less often. The sphere matters for the payment stage and for the value, in opposite directions. Caveats: the federal extension is read from ObrasGov planned against current end, the state one from the register's terms; carry-over is the same concept in both (paid as restos over paid); the federal base holds only contracts with payments from 2020.

### 6.3 Fiscal shock: does a state cut interrupt the contract, and does the channel protect it?

Contract-year panel of 18,858 observations (contracts active between their first and last payment year), outcome "no payment in the year", contract and year fixed effects, clustered by state-year (89 to 106 clusters). A cut is a real fall of 20% or 30% in the state's road payments.

| term | coefficient | t |
|---|---|---|
| cut of 30% in t, Treasury-type contracts | +3 to +4 pp on a 5% base | 2.3 to 2.7 |
| cut of 20% in t x Union transfer | -17 pp | -2.0 |
| cut of 20% in t-1 x Union transfer | -18 pp | -3.9 |
| cut of 20% in t+1 x Union transfer (placebo) | -4.5 pp | -0.7 |
| cut of 20% in t x credit-paid | +3.6 pp | 1.9 |
| cut of 20% in t+1 x credit-paid (placebo) | -6.8 pp | -3.3 |
| cut x multichannel, cut x fund | 0 | ns |

Reading. The state's fiscal cut interrupts its Treasury-paid contracts (the gap probability rises by two thirds). Contracts paid with Union transfers are insulated in the cut year and the year after, and the placebo lead is zero: this is the cleanest federative result of the sweep, with the caveat of few transfer contracts (390) and 29 cut events. Credit-paid contracts are not insulated; their lead placebo is significant with the wrong sign, which means the timing of gaps around cuts is not clean for credit (the counterpart is pulled forward or pushed back), so no claim is made for credit.

### 6.4 Infrastructure quality on federal highways

| outcome | treatment | design | coefficient | t |
|---|---|---|---|---|
| CNT overall score 2010-2018 | share of km conceded | asset and year FE | +0.20 | 2.2 |
| CNT signage score | share of km conceded | asset and year FE | +0.53 | 6.3 |
| CNT pavement, geometry | share of km conceded | asset and year FE | -0.13, -0.11 | ns |
| deaths per 100 km 2007-2025 | share of km inside a concession (staggered) | asset and year FE | -2.4 | -2.0 |
| deaths per 100 crashes (severity) | conceded stretch vs rest of the same highway, same year | highway x year FE | -2.6 | -2.3 |
| log deaths and log crashes per 100 km | conceded stretch vs rest of the same highway, same year | highway x year FE | +1.2, +1.9 | 6.6, 6.3 |
| CNT overall | log Union paid per km, same year / lag 2 | asset and year FE | -0.40 / +0.19 | -3.2 / 1.8 |
| DNIT ICM 2020-2026 | concession, Union paid | asset and year FE | 0 | ns |

Reading. Over time, when a highway's conceded share rises, its CNT score rises (signage above all) and deaths per km fall; in the cross-section the conceded stretch has far more crashes and deaths per km than the rest of the same highway, because it carries the traffic, and lower severity. Traffic is the confounder, and the staggered within-asset design is the only one that handles it. Federal spending per km is negative on condition in the same year (money goes to the bad road) and weakly positive two years later: the lag structure a spending-to-quality design would need. The DNIT maintenance index (2020-2026) is too short a panel to show anything yet.

### 6.5 Do the works change road safety on their own stretch?

Contract-stretch panel (1,479 federal contracts, deaths and crashes per 100 km on the km of the contract, 2007-2026), contract and year fixed effects, event time relative to signing. Nothing survives: no post-signing effect for all works, for capacity expansion (which shows rising deaths before the works, a selection signal), for maintenance, for extended or not-extended contracts. The one suggestive cell, crashes falling 3+ years after signing where carry-over exceeds 25% (t -2.5), has no pre-trend check that passes and is not reported as a finding.

### 6.6 What the sweep settles, ranked

1. The sphere of the money matters for delivery continuity: a state fiscal cut interrupts Treasury-paid contracts, Union-transfer contracts are insulated, with a clean placebo (6.3).
2. The sphere of the contract matters for the payment stage: federal direct execution carries over 21 to 33 pp more of its payment and lasts longer, with less value growth in capacity expansion (6.2).
3. Within the same contract, the loan tranche pays faster than the Treasury tranche (section 2.B).
4. The type of credit matters across states: external credit shows lender discipline at the payment stage (lower carry-over, higher execution, fewer stoppages, no extension), domestic credit shows the programme-work pattern (more extension and value growth). Not identified within states.
5. The Union transfer fixes the value (20 pp less value growth) and ends the contract when it stops (more rescission).
6. Concession on the asset improves condition and reduces deaths over time, once traffic is handled by the within-asset design; needs the modern staggered estimator and traffic data before it is reported as an effect.
7. Multichannel is the strongest association in the base on every delay outcome and remains an association.
8. Works on a stretch do not show a safety effect at this granularity.

Limitations added by the sweep: 29 cut events and 390 transfer contracts behind result 1; cross-sphere measurement of extension behind result 2; the within-state contrast of credit types is not identified; the contract-stretch accident panel has selection in the pre-period; CNT ends in 2018.

## 7. Validation protocol and audit (21 October, night)

The eight hypotheses of section 6 were re-estimated under an explicit protocol (unit, treatment, comparison, outcome, window, mechanism, confounders, identifying assumption, main specification chosen by design, falsification, support). Scripts `codigo/50_protocolo_causal.py` (H1, H2, H3, H5, H8; wild cluster bootstrap by state, randomization inference over cut years, leave-one-state-out), `50b_protocolo_tranche_refino.py` (H2 against the regular Treasury source only, timing check), `51_protocolo_concessoes.py` (H6 with Callaway-Sant'Anna aggregation and asset bootstrap), `52_auditoria_causal_documento.py` (tables p14 to p18 and the bilingual document `auditoria_causal.html` and `.md`). The sweep tables p05 to p08 are unchanged.

What the protocol changed:
- H1 (transfers protect from fiscal cuts): with the cut defined on the state's non-federal payments and state-by-year fixed effects, -10.4 pp (t -1.4; wild bootstrap p 0.44; randomization p 0.14); zero without Santa Catarina; the 1b-only split rests on 21 contracts. Reclassified from "cleanest federative result" to not identified (fragile). The lead placebo still passes.
- H2 (loan tranche): in SP the loan note carries over 17.5 pp less than the regular Treasury note of the same contract-year (t -5.4, 127 contracts, 2010-2016 only); the source placebo (Treasury 041 vs 001, +36 pp) showed that sources carry their own mechanics, so the main specification was restricted to source 001; the order of the notes biases against the result. In MG the effect on the share paid within the year disappears with commitment-month fixed effects: loan notes are issued later and paid at once. Classified suggestive (SP), not identified (MG).
- H5 (Union transfer and value growth): -21 pp (t -3.3), 1b instruments -32 pp, CIDE -11 pp, exact cells -18.5 pp, without Santa Catarina -8.7 pp, wild bootstrap by state p 0.19. Suggestive, mechanism-consistent, support concentrated.
- H6 (concessions): Callaway-Sant'Anna with never-treated controls: deaths per 100 km fall from year 3 (-2.3 to -5.5) but rise in years -3 and -2 before adoption (pre mean +1.1, se 0.55); severity post -2.0 (se 0.76) with a flat pre mean but a significant lead at -3; CNT pre and post equal (+0.21, +0.29). Treated assets had 31 deaths per 100 km before adoption against 6 among controls. Not identified (deaths, CNT); weakly suggestive (severity).
- H3, H7: robust associations, no identification. H4: not identified within states. H8: no post effect; joint pre-trend tests p 0.23 to 0.66.

Recommendation: the thesis is viable as a measurement-and-mechanism thesis with one causal module (the within-contract tranche comparison, to be extended with the month of the note and the 2022+ SP sheet) and H5 as a second module; concessions and fiscal shocks stay exploratory with named data needs. Document: https://claude.ai/artifact/L7kfmCn64yu1bw3aZbJEaY

## 8. Extensões pré-especificadas (22 out 2026; scripts 53, 53b, 53c, 53d, 53e; auditoria_causal, seção 9)

- **H2 em quatro estados (A).** A tranche do empréstimo carrega menos restos que a tranche regular do Tesouro no mesmo contrato-ano em SP (−16,5 pp, t −5,3, 138 contratos; 2010–2016 −21,8 pp) e em GO com efeitos contrato × ano × mês do empenho (−36 pp, t −3,0, 39 contratos; bootstrap por contrato p 0,005; sem diferença de calendário entre as notas). Agrupado SP+GO −22,6 pp (t −6,1; wild p 0,001); tirar qualquer contrato nunca muda o sinal. MG (9 de 498 notas com restos) e RS (linhas de restos sem ligação ao empenho) não entram por falta de suporte do resultado, não pelo resultado. H2 passa de sugestivo a identificado dentro do desenho, em dois estados e dois períodos; a frase fica no estágio de pagamento.
- **H2 federal (B).** 4.221 notas do DNIT de 803 contratos (2021–2024) em contratos-ano com duas ou mais fontes: notas de recursos livres (000/100) carregam 38% em restos; títulos −9,8 pp, CIDE −16,8 pp, outras vinculadas −5,8 pp (mesma direção dos estados). Sugestivo: sem a data da nota (SIAFI dá em 7%), o efeito dos títulos troca de sinal com o controle de ordem (2023, fim de ano); CIDE e vinculadas se apoiam em 2022; emenda é calendário. Próximo passo de dados: data de cada nota (Portal da Transparência) e a especificação de GO na União.
- **H1 com choque de RCL (C).** Queda real ≥5% da RCL: 5 estados-ano no painel; −2,0 pp (ep 3,9), wild p 0,61, permutação p 0,77, placebo adiantado do mesmo tamanho. Via fechada com estes dados.
- **H6 com controles comparáveis (D).** 45 controles nunca concedidos na faixa de mortes dos tratados; pré-tendências planas. Mortes por 100 km: −0,14 (ep 0,84), nulo (descarta efeitos acima de 14% da base). Mortes por 100 acidentes: −1,9 (ep 0,65) nos três desenhos, mas a coorte de 2014 carrega quase tudo (sem ela −0,46) e a contagem de acidentes é ruidosa demais para excluir efeito de registro. Sugestivo.
- Veredito depois das extensões: um resultado cumpre os dez critérios dentro do seu estimando (H2, SP e GO); H5 e H6 severidade sugestivos; H1, H4, H8 não identificados. Tabelas p19–p22b; nada anterior foi substituído.

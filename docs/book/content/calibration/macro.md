(Chap_MacroCalib)=
# Calibration of Macroeconomic Parameters

## Economic Assumptions

As the default rate of labor augmenting technological change, $g_y$, we use a value of 6%.  The average annual growth rate in GDP per capita in Ethiopia between 2006 and 2024 is 6.0% per year, according to [data from the World Bank](https://data.worldbank.org/indicator/NY.GDP.PCAP.KD.ZG?locations=ET).

## Open Economy Parameters

### Foreign holding of government debt in the initial period

The path of foreign holding of domestic debt is endogenous, but the initial period stock of debt held by foreign investors is exogenous.  We set this parameter, `initial_foreign_debt_ratio` to 0.95, consistent with [this report from the Ministry of Finance](https://www.mofed.gov.et/media/filer_public/9b/92/9b9264db-1a2b-4cd5-aa7d-f0307d67b4ce/public_sector_debt_statistical_bulletin_no_50.pdf).

### Foreign purchases of newly issued debt

We set $\zeta_D = 0.95$, the same as initial holdings of government debt by foreigners.

### Foreign holdings of excess capital

We set $\zeta_K = 0.20$.  Foreign capital plays a small role in financing Ethiopia's productive capital stock.  FDI inflows averaged 2 to 3 percent of GDP between 2018 and 2024 according to the [UNCTAD World Investment Report](https://unctad.org/system/files/official-document/wir2024_en.pdf), against gross fixed capital formation of roughly 25 percent of GDP per [World Bank NE.GDI.FTOT.ZS](https://data.worldbank.org/indicator/NE.GDI.FTOT.ZS?locations=ET); the implied foreign-financing share of new capital is 8 to 12 percent.  The [IMF Annual Report on Exchange Arrangements and Exchange Restrictions](https://www.imf.org/en/Publications/Annual-Report-on-Exchange-Arrangements-and-Exchange-Restrictions) documents binding NBE controls on foreign currency outflows, queued dividend repatriation, and no resident access to international portfolio markets, and foreign ownership of banks was prohibited until the 2024 financial-sector reform.  We set $\zeta_K$ slightly above the empirical foreign-financing share to allow for ongoing capital account liberalization without overstating the de facto openness of the economy.

## Government Debt, Spending and Transfers

### Government Debt

The path of government debt is endogenous. But the initial value and the steady-state (long-run) value are exogenous. To avoid converting between model units and dollars, we calibrate the initial debt to GDP ratio, rather than the dollar value of the debt. This is the model parameter $\alpha_D$ and the parameter name in [`ogeth_default_parameters.json`](https://github.com/EAPD-DRB/OG-ETH/blob/main/ogeth/ogeth_default_parameters.json) is `initial_debt_ratio`.  We compute this from the ratio of publicly held debt outstanding to GDP. Based on the 2019 value reported by the World Bank, the initial debt-to-GDP ratio in Ethiopia is 0.314.[^macro_wb_DY]

The steady-state debt-to-GDP ratio is set as `debt_ratio_ss = 0.40`.  This anchors the long-run debt level to the [IMF Country Report 25/188](https://www.imf.org/en/publications/cr/issues/2025/07/15/the-federal-democratic-republic-of-ethiopia-2025-article-iv-consultation-third-review-under-568611) medium-term fiscal framework (Table 1), which projects Ethiopia's public-debt-to-GDP ratio declining from a 49.8 percent peak in FY2024/25 to 34.5 percent by FY2028/29.  The value sits well below the 55 percent threshold at which low-income countries are classified as at high risk of debt distress under the IMF/World Bank Debt Sustainability Framework.


#### Interest rates on government debt

We assume that there is a wedge between the real rate of return on private capital and the real interest rate on government debt.  We model this wedge a scale and level shift.  Specifically, we assume that the real interest rate on government debt, $r_{gov,t}$, is related to the real rate of return on private capital, $r_{t}$, by the following equation:

```{math}
:label: eqn:r_gov
    r_{gov,t} = (1-\tau_{d,t})r_t + \mu_d
```

where $\tau_d$ is the scale parameter and $\mu_d$ is the level shift parameter.  We set the values of these two parameters to 0.245 and -0.034, respectively.  These are found by using the estimated relationship between corporate and sovereign yields in {cite}`LMW2023` (Table 8, Column 2) and simulating a series of corporate yields given a series of sovereign yields between 2% and 12%.  We then estimate the scale and level shift parameters that best fit these simulated data using ordinary least squares.

### Aggregate transfers

Aggregate (non-Social Security) transfers to households are set as a share of GDP with the parameter $\alpha_T$. We exclude Social Security from transfers since it is modeled specifically. With this definition, the share of transfers to GDP in 2015 is 0.034 according to [IMF data](https://data.imf.org/en/Data-Explorer?datasetUrn=IMF.STA:GFS_SOO(12.0.0)&INDICATOR=G271_T).

### Government expenditures

Government consumption is set as a share of GDP with the parameter $\alpha_G$. We use the World Bank series [General government final consumption expenditure (% of GDP)](https://data.worldbank.org/indicator/NE.CON.GOVT.ZS?locations=ET) (`NE.CON.GOVT.ZS`), which captures current spending on goods and services and excludes capital outlays.  The most recent (2024) value is 5.52 percent of GDP.  We set $\alpha_G = 0.055$.  Public infrastructure investment is calibrated separately via $\alpha_I$ (see the firm calibration), so the two parameters do not overlap.


(SecLWI_footnotes)=
## Footnotes
The following are the footnotes for this section.

[^macro_wb_DY]: See https://data.worldbank.org/country/ethiopia, accessed Nov. 17, 2025.

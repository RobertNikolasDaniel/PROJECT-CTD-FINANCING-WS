# Student FV Basket CTD Analyzer

## Purpose

This project was built to explore the financing economics behind U.S. Treasury futures delivery baskets.

Rather than simply identifying the current Cheapest-to-Deliver (CTD) bond, the workbook calculates a fair-value futures price for every deliverable bond in the basket and compares:

* The **Ideal CTD** implied by financing economics.
* The **Market CTD** implied by the current futures price.
* The difference between **Ideal Net Basis** and **Market Net Basis**.

The goal is to build intuition around:

* Treasury futures delivery mechanics.
* Basket financing economics.
* Repo funding effects.
* Conversion factors.
* Accrued interest.
* Why the market CTD can differ from the financing-implied CTD.
* Richness and cheapness between futures and delivery economics.

This is an educational project and is intended to help visualize how financing assumptions influence CTD selection.

---

# Project Logic

For each deliverable bond in the basket:

1. Calculate accrued interest.
2. Calculate dirty price.
3. Project accrued interest to delivery.
4. Calculate repo financing cost.
5. Calculate a financing-implied fair value futures price.
6. Calculate an Ideal Net Basis using the fair value futures price.
7. Calculate a Market Net Basis using the actual market futures price.
8. Rank all bonds by net basis.
9. Identify:

   * Ideal CTD
   * Market CTD
10. Compare the two selections and evaluate richness/cheapness.

---

# Inputs

| Input  | Description                                |
| ------ | ------------------------------------------ |
| CUSIP  | Bond identifier                            |
| Repo   | Financing rate used for carry calculations |
| CF     | CME Conversion Factor                      |
| DTD    | Days to Delivery                           |
| Par    | Bond par value                             |
| Clean  | Clean bond price                           |
| Rate   | Coupon rate                                |
| Freq   | Coupon frequency                           |
| Since  | Days since last coupon payment             |
| Period | Days in coupon period                      |

---

# Formulas

## Coupon Payment

Coupon payment received per coupon period.

[
Payment = \frac{Rate \times Par}{Freq}
]

---

## Accrued Interest

Current accrued interest.

[
AI = \left(\frac{Since}{Period}\right) \times Payment
]

---

## Dirty Price

Total economic purchase price.

[
Dirty = Clean + AI
]

---

## Projected Accrued Interest

Projected accrued interest at delivery.

[
ProjAI = AI + \left(\frac{DTD}{Period}\right) \times Payment
]

---

## Repo Financing Cost

Cost of carrying the bond until delivery.

[
RepoCost = Dirty \times Repo \times \left(\frac{DTD}{360}\right)
]

---

## Fair Value Futures Price

Financing-implied breakeven futures price.

[
FV = \frac{Dirty + RepoCost - ProjAI}{CF}
]

This represents the futures price implied by the financing economics of the bond.

---

## Converted Futures Price

Futures price adjusted using the CME conversion factor.

[
Converted = Futures \times CF
]

---

## Invoice Price

Expected delivery proceeds.

[
Invoice = Converted + ProjAI
]

---

## Gross Basis

Difference between cash bond value and converted futures value.

[
GrossBasis = Dirty - Converted
]

---

## Net Basis

Difference between cash bond value and expected delivery proceeds.

[
NetBasis = Dirty - Invoice
]

Lower values indicate more attractive delivery economics.

---

# CTD Selection Logic

The Cheapest-to-Deliver bond is defined as:

[
CTD = \min(NetBasis)
]

The bond with the lowest net basis is selected as the delivery candidate.

---

# Ideal CTD

The Ideal CTD is calculated using:

* Fair Value Futures Price
* Financing assumptions
* Repo economics

This represents the bond that should be cheapest to deliver according to the model.

---

# Market CTD

The Market CTD is calculated using:

* Current market futures price

This represents the bond that is currently cheapest to deliver according to market pricing.

---

# Rich / Cheap Analysis

A richness and cheapness measure is calculated using:

[
BasisSpread = MarketNetBasis - IdealNetBasis
]

Interpretation:

* Positive → Rich
* Negative → Cheap
* Near Zero → Fair

This allows comparison between market pricing and financing-implied pricing.

---

# Educational Takeaways

This project demonstrates that:

* CTD selection is not solely a function of conversion factors.
* Repo financing directly impacts delivery economics.
* Accrued interest materially affects invoice value.
* Different financing assumptions can produce a different CTD.
* Market CTD and financing-implied CTD are not always the same bond.
* Treasury futures pricing is closely connected to cash bond financing.

The project is designed as a student tool for learning Treasury futures delivery mechanics, basis analysis, financing economics, and CTD selection.

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

```text
Coupon Payment = (Coupon Rate × Par Value) / Coupon Frequency
```

---

## Accrued Interest

Current accrued interest since the last coupon payment.

```text
Accrued Interest = (Days Since Last Coupon / Days In Coupon Period)
                   × Coupon Payment
```

---

## Dirty Price

Total economic purchase price of the bond.

```text
Dirty Price = Clean Price + Accrued Interest
```

---

## Projected Accrued Interest

Expected accrued interest at delivery.

```text
Projected Accrued Interest = Current Accrued Interest
                             + (Days To Delivery / Days In Coupon Period)
                             × Coupon Payment
```

---

## Repo Financing Cost

Estimated financing cost of carrying the bond until delivery.

```text
Repo Cost = Dirty Price
            × Repo Rate
            × (Days To Delivery / 360)
```

---

## Fair Value Futures Price

Financing-implied breakeven futures price.

```text
Fair Value Futures Price =
(Dirty Price + Repo Cost - Projected Accrued Interest)
÷ Conversion Factor
```

---

## Converted Futures Price

Futures price adjusted using the CME conversion factor.

```text
Converted Futures Price =
Futures Price × Conversion Factor
```

---

## Invoice Price

Expected delivery proceeds.

```text
Invoice Price =
Converted Futures Price + Projected Accrued Interest
```

---

## Gross Basis

Difference between cash bond value and converted futures value.

```text
Gross Basis =
Dirty Price - Converted Futures Price
```

---

## Net Basis

Difference between cash bond value and expected delivery proceeds.

```text
Net Basis =
Dirty Price - Invoice Price
```

Lower values indicate more attractive delivery economics.

---

## CTD Selection

The Cheapest-to-Deliver bond is identified as:

```text
CTD = Lowest Net Basis
```

---

## Rich / Cheap Analysis

Difference between market pricing and financing-implied pricing.

```text
Basis Spread =
Market Net Basis - Ideal Net Basis
```

Interpretation:

```text
Positive = Rich
Negative = Cheap
Zero     = Fair Value
```


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

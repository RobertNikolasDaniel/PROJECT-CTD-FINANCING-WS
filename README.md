# Student FV Basket CTD Analyzer

## What Is This?

This project is a simple educational workbook that explores how Treasury futures delivery baskets work.

The workbook calculates a financing-implied fair value futures price for every bond in a delivery basket, then compares that result against the actual market futures price.

The goal is to answer a simple question:

> Does the bond that should be cheapest to deliver match the bond that the market is currently choosing?

---

# Why I Built This

When first learning Treasury futures, CTD selection often looks simple:

```text
Lowest Net Basis = CTD
```

After building this project, I realized there is more going on.

Things like:

* Repo financing
* Accrued interest
* Conversion factors
* Days to delivery

all influence delivery economics.

This workbook was built to help visualize those relationships.

---

# Inputs

Each bond requires:

| Input  | Description            |
| ------ | ---------------------- |
| CUSIP  | Bond identifier        |
| Repo   | Financing rate         |
| CF     | Conversion factor      |
| DTD    | Days to delivery       |
| Par    | Bond face value        |
| Clean  | Clean price            |
| Rate   | Coupon rate            |
| Freq   | Coupon frequency       |
| Since  | Days since last coupon |
| Period | Days in coupon period  |

---

# Process

For every bond in the basket:

### 1. Calculate Coupon Payment

```text
Coupon Payment = (Rate × Par) ÷ Frequency
```

### 2. Calculate Accrued Interest

```text
Accrued Interest = (Since ÷ Period) × Coupon Payment
```

### 3. Calculate Dirty Price

```text
Dirty Price = Clean Price + Accrued Interest
```

### 4. Estimate Accrued Interest At Delivery

```text
Projected AI =
Current AI + (DTD ÷ Period) × Coupon Payment
```

### 5. Calculate Financing Cost

```text
Repo Cost =
Dirty Price × Repo × (DTD ÷ 360)
```

### 6. Calculate Fair Value Futures Price

```text
Fair Value Futures Price =
(Dirty Price + Repo Cost − Projected AI)
÷ Conversion Factor
```

### 7. Calculate Delivery Economics

```text
Converted Futures =
Futures Price × Conversion Factor
```

```text
Invoice Price =
Converted Futures + Projected AI
```

```text
Gross Basis =
Dirty Price − Converted Futures
```

```text
Net Basis =
Dirty Price − Invoice Price
```

---

# CTD Logic

The workbook defines CTD as:

```text
CTD = Lowest Net Basis
```

Lower net basis values represent more attractive delivery economics.

---

# Two CTDs

The workbook calculates two different CTDs.

### Ideal CTD

Uses the financing-implied fair value futures price.

This answers:

> Which bond should be cheapest to deliver according to the model?

---

### Market CTD

Uses the actual market futures price.

This answers:

> Which bond is currently cheapest to deliver according to the market?

---

# Rich / Cheap Analysis

The workbook compares:

```text
Market Net Basis
```

against

```text
Ideal Net Basis
```

using:

```text
Basis Spread =
Market Net Basis − Ideal Net Basis
```

Interpretation:

```text
Positive = Rich

Negative = Cheap

Zero = Fair Value
```

---

# Main Question

The entire project is built around one question:

```text
Does the Market CTD match the Ideal CTD?
```

If the answer is no, the workbook helps visualize why.

---

# Lessons Learned

Building this project helped reinforce a few ideas:

* Treasury futures are financing instruments as much as they are duration instruments.
* Repo matters.
* Accrued interest matters.
* Conversion factors matter.
* Delivery economics matter.
* The market CTD is not always the same as the financing-implied CTD.

Most importantly:

```text
CTD selection is a financing problem.
```

This workbook was built as a student project to better understand Treasury futures delivery mechanics and basket financing.

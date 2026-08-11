# Sample Test Credit Card Numbers

Synthetic card numbers for testing payment forms, validation logic, and DLP
detection rules. Every number here is a publicly documented test value from
payment-processor sandbox documentation (Stripe, Adyen, Braintree, PayPal,
Worldpay). They pass the Luhn checksum and match real brand prefixes, so
validators and PAN-detection scanners treat them as genuine — but they are not
issued to anyone and will be declined by every live payment network.

Do not use them against production endpoints.

## Visa

| Number | Length | Notes |
| --- | --- | --- |
| 4111111111111111 | 16 | The canonical Visa test number |
| 4012888888881881 | 16 | Visa, alternate |
| 4222222222222 | 13 | Legacy 13-digit Visa |
| 4000056655665556 | 16 | Visa debit |
| 4005519200000004 | 16 | Visa, Worldpay sandbox |
| 4917610000000000 | 16 | Visa Electron |

## Mastercard

| Number | Length | Notes |
| --- | --- | --- |
| 5555555555554444 | 16 | The canonical Mastercard test number |
| 5105105105105100 | 16 | Mastercard, alternate |
| 2223003122003222 | 16 | 2-series BIN range (2221–2720) |
| 2223000048400011 | 16 | 2-series BIN range, alternate |
| 5200828282828210 | 16 | Mastercard debit |
| 6759649826438453 | 16 | Maestro |

## American Express

| Number | Length | Notes |
| --- | --- | --- |
| 378282246310005 | 15 | The canonical Amex test number |
| 371449635398431 | 15 | Amex, alternate |
| 378734493671000 | 15 | Amex Corporate |

## Discover

| Number | Length | Notes |
| --- | --- | --- |
| 6011111111111117 | 16 | The canonical Discover test number |
| 6011000990139424 | 16 | Discover, alternate |
| 6011981111111113 | 16 | Discover, alternate |

## Diners Club

| Number | Length | Notes |
| --- | --- | --- |
| 3056930009020004 | 16 | Diners Club |
| 36227206271667 | 14 | Diners Club International |
| 38520000023237 | 14 | Diners Club, legacy 14-digit |

## JCB

| Number | Length | Notes |
| --- | --- | --- |
| 3566002020360505 | 16 | The canonical JCB test number |
| 3530111333300000 | 16 | JCB, alternate |
| 3337000000000008 | 16 | JCB, alternate |

## UnionPay

| Number | Length | Notes |
| --- | --- | --- |
| 6200000000000005 | 16 | UnionPay credit |
| 6205500000000000004 | 19 | UnionPay, 19-digit PAN |

## Other brands

| Number | Length | Notes |
| --- | --- | --- |
| 5067268650517446 | 16 | Elo (Brazil) |
| 5019717010103742 | 16 | Dankort (Denmark) |

## Supporting test values

Sandbox environments generally accept any well-formed values for the remaining
fields:

- **Expiry date** — any future month/year, e.g. `12/2030`. Use a past date such
  as `01/2020` to exercise expired-card handling.
- **CVV / CVC** — any 3 digits (`123`), or 4 digits for Amex (`1234`).
- **Cardholder name** — any non-empty string.
- **Postal code** — `10001`, or `SW1A 1AA` for UK AVS checks.

## Declined-card test numbers

Processor sandboxes reserve specific numbers to force failure paths. These are
Stripe's; other processors publish their own equivalents.

| Number | Simulated outcome |
| --- | --- |
| 4000000000000002 | Generic decline |
| 4000000000009995 | Insufficient funds |
| 4000000000009987 | Lost card |
| 4000000000009979 | Stolen card |
| 4000000000000069 | Expired card |
| 4000000000000127 | Incorrect CVC |
| 4000000000000119 | Processing error |

## Checksum validation

Every number on this page — including the declined set — validates under the
Luhn algorithm:

```python
def luhn_valid(pan: str) -> bool:
    digits = [int(c) for c in pan if c.isdigit()][::-1]
    total = 0
    for i, d in enumerate(digits):
        if i % 2 == 1:
            d *= 2
            if d > 9:
                d -= 9
        total += d
    return total % 10 == 0
```

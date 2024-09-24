---
title: "Quick post on Bayes Theorem"
tags: [probability]
---

# Bayes Theorem  

$$P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$$

Where:
- \( P(A|B) \): The probability of event \( A \) occurring given that \( B \) has occurred (this is called the "posterior").
- \( P(B|A) \): The probability of event \( B \) occurring given that \( A \) has occurred (called the "likelihood").
- \( P(A) \): The prior probability of event \( A \) occurring (before we know \( B \)).
- \( P(B) \): The total probability of event \( B \) occurring (the normalizing constant, which accounts for all possible ways \( B \) could happen).

### Scenario:

- **\( A \)**: Your friend responds quickly to a text message (within 5 minutes).
- **\( B \)**: You sent a message in the morning.

You want to know the probability that your friend will respond quickly (\( A \)) given that you sent the message in the morning (\( B \)).

### Given Data (Realistic Assumptions):

1. **\( P(A) \)**: The probability that your friend responds quickly to a message in general is 20%, so \( P(A) = 0.20 \).
2. **\( P(B|A) \)**: If your friend responds quickly, there's a 70% chance that it happens in the morning. So, \( P(B|A) = 0.70 \).
3. **\( P(B) \)**: The general probability that you send a message in the morning is 50%, so \( P(B) = 0.50 \).

### Question:

What is the probability that your friend will respond quickly, given that you sent the message in the morning? We are looking for \( P(A|B) \).

### Apply Bayes' Theorem:

$$
P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}
$$

Substituting the known values:

$$
P(A|B) = \frac{0.70 \cdot 0.20}{0.50}
$$

$$
P(A|B) = \frac{0.14}{0.50} = 0.28
$$

### Conclusion:

The probability that your friend will respond quickly, given that you sent the message in the morning, is **28%**.


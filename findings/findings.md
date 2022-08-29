**Severity:** *Critical Risk* / *High Risk* / *Medium Risk* / *Low Risk* / *Informational* / *Gas Optimization*
___

**Severity:** *Gas Optimization*

**Context:** [INSTR.sol#L44](https://github.com/code-423n4/2022-08-olympus/blob/b5e139d732eb4c07102f149fb9426d356af617aa/src/modules/INSTR.sol#L44), [PRICE.sol#144](https://github.com/code-423n4/2022-08-olympus/blob/b5e139d732eb4c07102f149fb9426d356af617aa/src/modules/PRICE.sol#L144)


**Description:** Arithmetic checks aren't necessary when logic cannot realistically underflow/overflow.

**Recommendation:** 
[INSTR.sol#L44](https://github.com/code-423n4/2022-08-olympus/blob/b5e139d732eb4c07102f149fb9426d356af617aa/src/modules/INSTR.sol#L44)
```solidity
-    uint256 instructionsId = ++totalInstructions;
+    uint256 instructionsId;
+
+    unchecked {
+        instructionsId = ++totalInstructions;
+    }
```

**Recommendation:** 
[PRICE.sol#144](https://github.com/code-423n4/2022-08-olympus/blob/b5e139d732eb4c07102f149fb9426d356af617aa/src/modules/PRICE.sol#L144)
```solidity
+    uint256 nextObs = nextObsIndex;

...

-    nextObsIndex = (nextObsIndex + 1) % numObs;
+
+    unchecked {
+        ++nextObs;
+    }
+
+    nextObsIndex = nextObs % numObs;
```

**Recommendation:** 
[PRICE.sol#135](https://github.com/code-423n4/2022-08-olympus/blob/b5e139d732eb4c07102f149fb9426d356af617aa/src/modules/PRICE.sol#L135)
```solidity
    if (currentPrice > earliestPrice) {
-        _movingAverage += (currentPrice - earliestPrice) / numObs;
+
+        unchecked {
+            priceDelta = currentPrice - earliestPrice;
+        }
+
+        _movingAverage += priceDelta / numObs;
    } else {
-        _movingAverage -= (earliestPrice - currentPrice) / numObs;
+
+        unchecked {
+            priceDelta = earliestPrice - currentPrice;
+        }
+
+        _movingAverage -= priceDelta / numObs;
+    }
```

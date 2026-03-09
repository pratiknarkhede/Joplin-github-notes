### **Time Complexity Guide for Constraints**

**Purpose**: Helps determine acceptable algorithms in CP based on problem constraints.

| **Constraint (N)** | **Acceptable Time Complexity** | **Mathematical Term** |
| --- | --- | --- |
| N ≤ 15 | O(N!) | Factorial |
| N ≤ 30 | O(Xᴺ), X = constant | Exponential |
| N ≤ 10² (100) | O(N³) | Cubic |
| N ≤ 10³ (1,000) | O(N²) always; O(N³) *sometimes* | Quadratic, Cubic |
| N ≤ 10⁴ (10,000) | O(N²) *with low constants* | Quadratic |
| N ≤ 5×10⁴ (50,000) | O(NlogN), O(N√N) | Linearithmic, Linear × Square Root |
| N ≤ 10⁵ (100,000) | O(NlogN), O(N√N) | Linearithmic, Linear × Square Root |
| N ≤ 10⁶ (1,000,000) | O(NlogN) | Linearithmic |
| N ≤ 10⁷ (10,000,000) | O(N) | Linear |
| N ≤ 10⁸ (100,000,000) | O(N) | Linear |
| N ≤ 10⁹ (1,000,000,000) | O(√N) | Square Root |
| N ≥ 10⁹ | O(logN) or O(1) | Logarithmic, Constant |
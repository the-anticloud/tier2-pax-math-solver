# Deploy Guide — PAX_MATH_SOLVER
**Stack:** Python 3.11, sympy, scipy, numpy, PAX 27B, AIOSS_FORMAT | Air-gap capable

## Prerequisites
Anticloud core stack installed. PAX 27B weights (pax-27b-q4.gguf). AIOSS_FORMAT.

## Install
```bash
pip install anticloud-pax-math-solver
```

## AIOSS Integration
```bash
aioss init --module PAX_MATH_SOLVER --output ./pax_math_solver.aioss
```

## Air-Gap
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(model_path="./pax-27b-q4.gguf", module="PAX_MATH_SOLVER",
                     aioss_chain="./pax_math_solver.aioss",
                     classification="L5_NARROW_L2_GENERAL")
```

## Verification
```bash
aioss verify --chain ./pax_math_solver.aioss --verbose
python -m pax_math_solver.tests.smoke
```

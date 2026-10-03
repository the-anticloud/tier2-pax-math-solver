# Developer Cookbook — PAX_MATH_SOLVER
**Stack:** Python 3.11, sympy, scipy, numpy, PAX 27B, AIOSS_FORMAT

## Basic Usage
```python
from pax_math_solver import Mathsolver
module = Mathsolver(pax_model="./pax-27b-q4.gguf",
                               aioss_chain="./pax_math_solver.aioss")
result = module.process(input_data)
print(result.output, result.chain_hash)
```

## Batch Processing
```python
results = module.process_batch(inputs, batch_size=4)
for r in results:
    print(r.chain_hash)
```

## AIOSS Append
```python
import hashlib, time
def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()

chain_hash = aioss_append("./pax_math_solver.aioss", result.to_bytes(), "PAX_MATH_SOLVER")
```

## Integration with Anticloud TIER_2
```python
# Chain with PAX_INFERENCE_CORE
from pax_inference_core import PAXInferenceCore
from pax_math_solver import Mathsolver

core = PAXInferenceCore(model="./pax-27b-q4.gguf")
module = Mathsolver(inference_core=core)
```

## Domain: Symbolic math and numerical computation module for PAX 27B
This module specializes in: symbolic math and numerical computation module for pax 27b.
AIOSS entry type: math solution (problem hash + solution steps hash + verification result).

## Performance
Use module.benchmark() to measure throughput on your hardware.
Pre-warm: module.warmup() before serving production requests.

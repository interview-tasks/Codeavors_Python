# Technical Assessment - AIOhttp & PyMongo Testing

## Quick Linux Test Execution

### Prerequisites
- Docker installed and running

### Run AIOhttp Tests in Linux Container

```bash
cd aiohttp

# Build the Docker test image
docker build --build-arg PYTHON_VERSION=3.10 --build-arg AIOHTTP_NO_EXTENSIONS=n -t aiohttp-test-3.10-n -f tools/testing/Dockerfile .

# Run all tests (includes 6 known failing circular import tests)
docker run --rm -v $(pwd):/src -w /src aiohttp-test-3.10-n

# Run tests excluding known failing circular import tests (RECOMMENDED)
docker run --rm -v $(pwd):/src -w /src aiohttp-test-3.10-n python -m pytest --deselect tests/test_circular_imports.py::test_no_warnings -q
```

### Run PyMongo Tests

```bash
cd mongo-python-driver

# Create virtual environment and install
python3 -m venv venv
source venv/bin/activate
pip install -e .

# Run basic functionality tests
python -c "
import sys
sys.path.insert(0, '.')
from bson import BSON, decode, encode
from bson.objectid import ObjectId
from bson.decimal128 import Decimal128

print('=== PyMongo Basic Tests ===')
print('✓ ObjectId:', ObjectId())
print('✓ BSON encode/decode:', decode(encode({'test': 42})))
print('✓ Decimal128:', Decimal128('123.456'))
print('All tests passed!')
"
```

### Expected Results
- **AIOhttp**: Exit code 0 when excluding circular import tests
- **PyMongo**: All basic functionality working

### Assessment Files
- `TECHNICAL_ASSESSMENT_RESULTS.txt` - Complete analysis
- `aiohttp_linux_test_results.txt` - Linux test results
- `mongo_manual_tests_output.txt` - PyMongo test output
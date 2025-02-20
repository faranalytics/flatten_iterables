# Flatten It

## Introduction

Flatten It uses iteration (no recursion) to flatten iterables into a dictionary where each key is a reference path and each value is the value at the respective path.

## Features

- Produces valid Python or JS reference paths for string and numeric keys.
- Raises `ValueError` on circular references.
- Iterative algorithm; flatten deeply nested structures without causing a call stack overflow.

## Table of contents

- [Installation](#installation)
- [Usage](#usage)
- [API](#api)
- [Test](#test)

## Installation

```bash
pip install flatten_iterables
```

## Usage

In this example, an object named `data` will be flattened into a dictionary of reference paths and their respective values. The resulting dictionary will be dumped to a JSON string.

```python
import json
from flatten_iterables import fi

fi.key_style = "python"

data = {
    "dict": {"a": 42},
    "nested_dicts": {"a0": {"b0": 42, "b1": 23}},
    "empty_dict": {},
    "list": [42],
    "nested_lists": [
        [
            42,
        ],
    ],
    "empty_list": [],
    ...: 42,
}

print(json.dumps(fi.flatten(data), indent=2))
```

```bash
{
  "['dict']['a']": 42,
  "['nested_dicts']['a0']['b0']": 42,
  "['nested_dicts']['a0']['b1']": 23,
  "['empty_dict']": {},
  "['list'][0]": 42,
  "['nested_lists'][0][0]": 42,
  "['empty_list']": [],
  "[Ellipsis]": 42
}
```

By default Flatten It will flatten structures that contain instances of `list` and `dict`. However, you can flatten structures containing other types of iterables and mappings by adding their respective types to the `iterables` and `mappables` sets.

In this example, a structure containing the types `set` and `OrderedDict` will be flattened. The type `set` is added to the `iterables` set and the type `OrderedDict` is added to the `mappables` set.

```python
import json
from flatten_iterables import fi
from collections import OrderedDict

fi.iterables.add(set)
fi.mappables.add(OrderedDict)

data = {
    "set": {23, 42, 57},
    "ordered_dict": OrderedDict(a=23, b=42, c=57),
}

print(json.dumps(fi.flatten(data), indent=2))
```

```
{
  "['set'][0]": 57,
  "['set'][1]": 42,
  "['set'][2]": 23,
  "['ordered_dict']['a']": 23,
  "['ordered_dict']['b']": 42,
  "['ordered_dict']['c']": 57
}
```

## API

**fi.flatten(it)**

- it `Union[Iterable, Mapping]` The iterable or mapping to be flattened.

**fi.key_style** `Literal["python", "js"]` Specify a key style. **Default:** `python`

**fi.iterables** `Set[Iterables]` Add iterable candidates to this set. **Default:** `{list}`

**fi.mappables** `Set[Mapping]` Add mappable candidates to this set. **Default:** `{dict}`

## Test

### How to run the test

#### Clone the repository.

```bash
git clone https://github.com/faranalytics/flatten_iterables.git
```

#### Change directory into the root of the repository.

```bash
cd flatten_iterables
```

#### Install the package in editable mode.

```bash
pip install -e .
```

#### Run the tests.

```bash
python -m unittest -v
```

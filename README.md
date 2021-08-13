# OpenAI-Codex-Solution

# Problem 1
```
Given a the contents of a CSV file as csv_contents, return the difference in days between the date of the earliest and the oldest entry.

The CSV file starts with a header row, which contains at least one column called Date.

You are optionally provided with the pandas library if you need it.

EXAMPLES
Input	"Date,Price,Volume\n2014-01-27,550.50,1387\n2014-06-23,910.83,4361\n2014-05-20,604.51,5870"
Output	147
Explanation	There are 147 days between 2014-01-27 and 2014-06-23.
```
# Solution 1
```
from datetime import date
import pandas
import io
import tempfile
import os
from pathlib import Path
days=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
def diff_days(csv_contents:str)->int:
    with tempfile.TemporaryDirectory() as td:
        fname=Path(td)/'bla.cvs'
        with fname.open('w') as f:
            f.write(csv_contents)
        data=pandas.read_csv(str(fname))
        earliest=min(data['Date'])
        latest=max(data['Date'])
    return ((date.fromisoformat(latest)-date.fromisoformat(earliest)).days)


# Examples
print(diff_days("Date,Price,Volume\n2014-01-27,550.50,1387\n2014-06-23,910.83,4361\n2014-05-20,604.51,5870"))
print(diff_days('Date\n2000-01-01\n2000-01-01\n'))

# The last expression evaluated is always shown when
# you run your code, just like a Jupyter notebook cell.
"Good luck!"

```

# Problem 2
```
source and target are two strings each containing file contents. Return the number of lines you would need to insert and the number of lines you would need to delete to get to target from source.

Return your answer as a tuple (inserted, deleted).

LIBRARY SUGGESTION
Consider using the difflib module.

EXAMPLES
Input	source = "Apple\nOrange\nPear" target = "Apple\nPear\nBanana\nMango"
Output	(2, 1)
```

# Solution2
```
import difflib
from typing import Tuple


import difflib
from typing import Tuple
from collections  import Counter

def diff_files(source: str, target: str) -> Tuple[int, int]:
    if source and  not source.endswith('\n'):
        source += '\n'
    if target and not target.endswith('\n'):
        target += '\n'
    diff =difflib.unified_diff(source.splitlines(keepends=True), target.splitlines(keepends=True),n=10)
    diff=list(diff)
    diff=[d[0] for d in diff[3:]]
    cts=Counter(diff)
    return cts['+'], cts['-']


# Examples
print(diff_files('Apple\nOrange\nPear', 'Apple\nPear\nBanana\nMango'))
print(diff_files('Apple\nOrange\nPear', 'Apple\nPear\nBanana'))
print(diff_files('0',''))
```
# Problem 3
```
Given a compressed message compressed and the prefix code tree used to encode it, decode and return the original message.

tree is a nested dictionary with characters at the leaves. For each character in compressed, traverse down one step of the tree. Once you reach a leaf of the tree, the character at that leaf is appended to the decoded message.

If the compressed message could not be decoded in full because the last step did not end at a leaf, return what was decoded so far.

CONSTRAINTS
The keys of tree and its subdictionaries are always '0' or '1', and the leaf values are also always single characters.

EXAMPLES
Input	compressed = "110100100" tree = {"0": "a", "1": {"0": "n", "1": "b"}}
Output	"banana"
Input	compressed = "0111010100" tree = {"0": {"0": "x", "1": "z"}, "1": "y"}
Output	"zyyzzx"
Input	compressed = "000" tree = {"0": {"0": "x", "1": "z"}, "1": "y"}
Output	"x"
```
# Solution3
```
from typing import Dict, Union

Tree = Dict[str, Union[str, "Tree"]]


def decompress(compressed: str, tree: Tree) -> str:
    result=''
    node=tree
    for bit in compressed:
        if isinstance(node,str):
            return result
        node=node[bit]
        if isinstance(node,str):
            result+=node
            node=tree
    return result

# Examples
print(decompress('110100100', {'0': 'a', '1': {'0': 'n', '1': 'b'}}))
print(decompress('0111010100', {'0': {'0': 'x', '1': 'z'}, '1': 'y'}))
```
# Problem 4
```
Parse the given Python source code and return the list of full-qualified paths for all imported symbols, sorted in ascending lexicographic order.

CONSTRAINTS
The input will not contain any wildcard imports (from ... import *).

Ignore aliases (renamings): from x import y as z should be represented as x.y.

LIBRARY SUGGESTION
Consider using the ast module.

EXAMPLES
Input

import os
import concurrent.futures
from os import path as renamed_path
from typing import (
    List, Tuple
)
Output

['concurrent.futures', 'os', 'os.path', 'typing.List', 'typing.Tuple']
```

# Solution4
```
import ast
from typing import List

def parse_imports(code: str) -> List[str]:
    tree = ast.parse(code)
    modules = []
    for node in ast.walk(tree):
        if isinstance(node, ast.Import):
            for child in node.names:
                modules.append(child.name)
            #print(modules)
        elif isinstance(node, ast.ImportFrom):
            for i in range(len(node.names)):

                x=node.module+'.'+node.names[i].name
                modules.append(x)

            #print(len(node.names))
    return sorted(modules)
    

# Examples
print(parse_imports('import os'))
print(parse_imports('import os\nfrom typing import List'))
```

# Problem 5
```
You have several buckets whose sizes are represented by the list sizes. Find the number of different ways to arrange the buckets such that the first bucket’s size is greater than the second bucket’s size.

CONSTRAINTS
sizes is a list of positive integers.

If sizes has fewer than 2 buckets, return 0.

Some buckets may have the same size, but are nevertheless treated as unique buckets.

LIBRARY SUGGESTION
Consider using the itertools module.

EXAMPLES
Input	[10]
Output	0
Explanation	No arrangement is possible.
Input	[1, 2]
Output	1
Explanation	The possible arrangements are (1, 2) and (2, 1). Of these, only (2, 1) has a first bucket that is bigger than the second bucket.
Input	[1, 3, 1]
Output	2
Explanation	Only the arrangements (3, 1, 1) and (3, 1, 1) satisfy the property that the first bucket is bigger than the second bucket.
```

# Solution 5

```
import itertools
from typing import List


def count_arrangements(sizes: List[int]) -> int:
    if len(sizes) < 2:
        return 0
    permutations = itertools.permutations(sizes)
    arrangements = 0
    for permutation in permutations:
        if permutation[0] > permutation[1]:
            arrangements += 1
    return arrangements
    

# Examples
print(count_arrangements([1, 3, 1]))
print(count_arrangements([1, 2]))
```


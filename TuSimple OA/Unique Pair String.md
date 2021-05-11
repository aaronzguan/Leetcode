## Unique Pair String (TuSimple OA)

Given you a string, if any pairs with the same distance in this string are different, it's a unique pair string.

For example. BEDF

1-distance pair: BE ED DF

2-distance pair: BD EF

3-distance pair: BF

because the pairs are distinctive in any distance, it's a unique pair string

But in the case "EEGFF" in the 3-distance pairs, "EF" appears twice, so it's a not unique pair string.

You will get a list of strings, and we want to know your check result for every string.

**Example:**

```
checking_string = ["BEDF", "EEGFF"]
result: [true, false]
```



### Brute Force

```python
class Solution():
    def check_unique_pair_string(self, original_strings):
        result = []
        for input_string in original_strings:
            result.append(self.isUniquePairString(input_string))
        return result

    def isUniquePairString(self, input_string):
        if len(input_string) <= 2:
            return True
        max_dist = len(input_string) - 1
        
        for dist in range(1, max_dist):
            seen = set()
            for i in range(len(input_string) - dist):
                temp_string = input_string[i] + "" + input_string[i + dist]
                if temp_string in seen:
                    return False
                seen.add(temp_string)
        return True
```


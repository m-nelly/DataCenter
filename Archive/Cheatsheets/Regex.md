Pulled From: [ihateregex.io](https://ihateregex.io/cheatsheet/#)
---
|expr|usage|
|---|---|
|`/hello/`|looks for the string between the forward slashes (case-sensitive)|
|`/hello/i`|looks for the string between the forward slashes (case-insensitive)|
|`/hello/g`|looks for multiple occurrences of string between the forward slashes|
|`/h.llo/`|the '.' matches any one character other than a new line character... matches 'hello', 'hallo' but not 'h  <br>llo'|
|`/h.*llo/`|the "\*" matches any character(s) zero or more times... matches "hello", "heeeeeello", "hllo", "hwarwareallo"|
|`/\d/` |matches any digit|
|`/\D/`|matches any non-digit|
|`/\w/`|matches any word character (a-z, A-Z, 0-9, \_)|
|`/\W/`|matches any non-word character|
|`/\s/`|matches any white space character (\r (carriage return),\n (new line), \t (tab), \f (form feed))|
|`/\S/`|matches any non-white space character|
|`/[abcd]/`|matches any character in square brackets|
|`/[ch]at/`|matches cat or hat|
|`/[^abcd]/`|matches anything except the characters in square brackets|
|`/[a-z]/`|matches all lowercase letters (a to z)|
|`/[A-Z]/`|matches all uppercase letters (A to Z)|
|`/[0-9]/`|matches all digits|
|`/[a-zA-Z]/`|matches all lowercase and uppercase letters|
|`/[^a-zA-Z]/`|matches non-letters|
|`/[a-zA-Z0-9]/`|matches all lowercase, uppercase letters and numbers|
|`/(hello){4}/`|matches "hellohellohellohello"|
|`/hello{3}/`|matches "hellooo" and "helloooo" but not "helloo"|
|`/(hello){1,3}/`|matches "hello" that occur between 1 and 3 times (inclusive)|
|`/(hello){3,}/`|matches "hello" that occur atleast 3 times|
|`/ab*c/`|matches zero or more repetitions of "b" (matches "abc", "abbbbc", "ac")|
|`/ab+c/`|matches one or more repetitions of "b" (matches "abc", "abbbbc", but not "ac")|
|`/^/`|matches beginning of a line|
|`/$/`|matches end of a line|
|`/(hard)?work/`|matches "work" or "hardwork"|
|`/(?:hard)?work/`|matches "work" or "hardwork" but is a non-capturing group|
|`/i am a (cat\|dog\|whale) person/`|matches "i am a cat person", "i am a dog person" and "i am a whale person"|
|`/z(?=a)/`|positive lookahead... matches the "z" before the "a" in pizza but not the first "z"|
|`/z(?!a)/`|negative lookahead... matches the first "z" but not the "z" before the "a"|
|`/(?<=[aeiou])\w/`|positive lookbehind... matches any word character that is preceded by a vowel|
|`/(?<![aeiou])\w/`|negative lookbehind... matches any word character that is not preceded by a vowel|
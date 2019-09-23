match
=====

[![Build Status](https://travis-ci.org/EducationalTestingService/match.svg?branch=master)](https://travis-ci.org/EducationalTestingService/match)

[![Latest Version](https://img.shields.io/pypi/v/match.svg)](https://pypi.python.org/pypi/match/)[![Downloads](https://img.shields.io/pypi/dm/match.svg)](https://pypi.python.org/pypi/match/)

The purpose of the module `Match` is to get the offsets (as well as the string between those offsets, for debugging) of a cleaned-up, tokenized string from its original, untokenized source.  "Big deal," you might say, but this is actually a pretty difficult task if the original text is sufficiently messy, not to mention rife with Unicode characters.

Consider some text, stored in a variable `original_text`, like:

```
I   am writing a letter !  Sometimes,I forget to put spaces (and do weird stuff with punctuation)  ?  J'aurai une pomme, s'il vous plâit !
```

This will/should/might be properly tokenized as:

```python
[['I', 'am', 'writing', 'a', 'letter', '!'],
 ['Sometimes', ',', 'I', 'forget', 'to', 'put', 'spaces', '-LRB-', 'and', 'do', 'weird', 'stuff', 'with', 'punctuation', '-RRB-', '?'],
 ["J'aurai", 'une', 'pomme', ',', "s'il", 'vous', 'pl\xe2it', '!']]
```

Now:

```python
In [22]: match.match(original_text, ['-LRB-', 'and', 'do', 'weird', 'stuff', 'with', 'punctuation', '-RRB-'])
Out[22]: [(60, 97, '(and do weird stuff with punctuation)')]

In [23]: Match.match(original_text, ['I', 'am', 'writing', 'a', 'letter', '!'])
Out[23]: [(0, 25, 'I   am writing a letter !')]

In [24]: Match.match(original_text, [u"s'il", 'vous', 'pl\xe2it', '!'])
Out[24]: [(121, 138, "s'il vous pl\xe2it !")]
```

The return type from `match()` is a `list` because it will return *all* occurrences of the argument, be it a `list` of tokens or a single `string` (word):

```python
In [25]: Match.match(original_text, "I")
Out[25]: [(0, 1, 'I'), (37, 38, 'I')]
```

When passing in a single `string`, `match()` is expecting that `string` to be a single word or token.  Thus, the following does not work as one would expect:

```python
In [5]: Match.match("****because,the****", "because , the")
Out[5]: []
```

Try `"because , the".split(" ")` instead.

For convenience, a function called `match_lines()` is provided:
```python
In [26]: Match.match_lines(original_text, [['-LRB-', 'and', 'do', 'weird', 'stuff', 'with', 'punctuation', '-RRB-'], ['I', 'am', 'writing', 'a', 'letter', '!'], "I"])
Out[26]: 
[(0, 1, 'I'),
 (0, 25, 'I   am writing a letter !'),
 (37, 38, 'I'),
 (60, 97, '(and do weird stuff with punctuation)')]
```

The values returned will always be sorted by their offsets.

## Installation

`pip install match`, or for Mac OS X and 64-bit Linux:

```
$ conda install -c dmnapolitano match
```

## Requirements

* Python >= 3.4
* [nltk](http://www.nltk.org)
* [regex](https://pypi.python.org/pypi/regex)

## Documentation

[Here!](match).

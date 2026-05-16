# 🌳 Trie Data Structure — C++ Implementation

A clean, from-scratch **Trie (Prefix Tree)** implementation in C++ supporting word insertion, search, prefix checking, and **autocomplete** — the same data structure behind search engines and keyboard suggestions.

---

## 📋 Table of Contents

- [Overview](#overview)
- [What is a Trie?](#what-is-a-trie)
- [Features](#features)
- [API Reference](#api-reference)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Test Coverage](#test-coverage)
- [Example Output](#example-output)

---

## Overview

This project implements the **Trie** (also called a Prefix Tree) data structure entirely from scratch in C++ with no external libraries. It focuses on four core operations: inserting words, searching for exact words, checking prefixes, and autocompleting based on a prefix — with built-in **case-insensitive** handling.

---

## 🔍 What is a Trie?

A Trie is a tree-shaped data structure where each path from the root to a marked node spells out a word. Each node stores up to 26 children — one per letter of the alphabet — making prefix-based lookups extremely fast.

```
         root
        /    \
       a      b
       |      |
       p      a
       |      |
       p      n
       |      |
       l    [ana]  ← "banana" ends here
       |
     [e]  ← "apple" ends here
```

- ✅ Insert: **O(L)** — L = length of word  
- ✅ Search: **O(L)**  
- ✅ Prefix check: **O(P)** — P = length of prefix  
- ✅ Autocomplete: **O(P + N)** — N = total characters in matching words

---

## ✨ Features

- 🔤 **Insert** words into the Trie
- 🔍 **Search** for exact words (returns `true`/`false`)
- 🔡 **Prefix check** — verify if any stored word starts with a given prefix
- 💡 **Autocomplete** — retrieve all words that begin with a given prefix
- 🔠 **Case-insensitive** — `"Hello"` and `"hello"` are treated as the same word
- ⚡ No external libraries — pure C++ Standard Library only

---

## 📖 API Reference

### `void insert(string word)`
Inserts a word into the Trie. Non-alphabetic characters are silently skipped. Case-insensitive.

```cpp
trie.insert("apple");
trie.insert("Hello");   // stored as "hello"
```

### `bool search(string word)`
Returns `true` if the **complete word** exists in the Trie.

```cpp
trie.search("apple");   // true
trie.search("app");     // false (prefix only, not a full word)
```

### `bool startsWith(string prefix)`
Returns `true` if **any stored word** begins with the given prefix.

```cpp
trie.startsWith("app");   // true  (apple, application...)
trie.startsWith("xyz");   // false
trie.startsWith("");      // true  (empty prefix matches everything)
```

### `vector<string> autocomplete(string prefix)`
Returns all words in the Trie that start with the given prefix. Returns an empty vector if no matches exist.

```cpp
trie.insert("apple");
trie.insert("application");
trie.insert("appetizer");

trie.autocomplete("app");
// → ["apple", "appetizer", "application"]
```

---

## 🧠 How It Works

### TrieNode Structure

```cpp
class TrieNode {
    TrieNode* children[26];   // One slot per letter a–z
    bool isEndOfWord;          // True if a word ends at this node
};
```

### Insert
Traverses character by character, creating new `TrieNode`s as needed, then marks `isEndOfWord = true` at the final node.

### Search
Traverses the same path as insert — returns `true` only if the path exists **and** the final node has `isEndOfWord = true`.

### startsWith
Same traversal as search, but returns `true` as soon as the full prefix path is found — regardless of `isEndOfWord`.

### Autocomplete
1. Navigates to the end of the prefix path
2. Calls `findAllWords()` recursively from that node
3. `findAllWords` does a DFS — appending each letter as it goes, and collecting the current word whenever `isEndOfWord` is `true`

```
prefix = "app"
         → navigate to node for 'p' in "app"
         → DFS from there:
              "apple"         ← isEndOfWord ✓
              "appetizer"     ← isEndOfWord ✓
              "application"   ← isEndOfWord ✓
```

### Case Handling
Both `insert` and `search` convert uppercase characters to their lowercase index (`c - 'A'`), treating the alphabet as case-insensitive.

---

## 📁 Project Structure

```
Trie/
│
├── main.cpp      # TrieNode class + Trie class + full test suite
└── .gitignore
```

---

## 🚀 Getting Started

**No dependencies required** — just a C++11-compatible compiler.

**Linux / macOS:**
```bash
g++ -std=c++11 -o Trie main.cpp
./Trie
```

**Windows (MinGW):**
```bash
g++ -std=c++11 -o Trie.exe main.cpp
Trie.exe
```

**Windows (MSVC):**
```bash
cl /EHsc /std:c++11 main.cpp /Fe:Trie.exe
```

---

## 🧪 Test Coverage

The `main()` function runs **6 test suites** automatically:

| # | Test | What it checks |
|---|------|----------------|
| 1 | Basic insert & search | Words inserted are found; partial matches are not |
| 2 | Prefix checking | Valid prefixes return `true`; non-existent ones return `false` |
| 3 | Autocomplete | Correct completions returned for various prefixes |
| 4 | Edge cases | Empty string search, empty prefix autocomplete |
| 5 | Additional words | Trie grows correctly; autocomplete returns multiple matches |
| 6 | Case sensitivity | `"Hello"` and `"hello"` resolve to the same node |

---

## 💡 Example Output

```
=== TRIE DATA STRUCTURE IMPLEMENTATION ===
Testing all Trie functionalities...

1. Testing basic insertion and search:
======================================
Inserted: apple
Inserted: banana
...
Search 'apple': FOUND
Search 'app': NOT FOUND (expected: NOT FOUND)

2. Testing prefix checking:
==========================
Prefix 'app': EXISTS
Prefix 'x': DOESN'T EXIST (expected: DOESN'T EXIST)

3. Testing autocomplete functionality:
======================================
Autocomplete for 'app': apple
Autocomplete for 'ban': banana

5. Testing with additional words:
================================
Autocomplete for 'app': apple, appetizer, application
Autocomplete for 'ban': banana, banister, bandana
Autocomplete for 'gra': grape, grapefruit

6. Testing case sensitivity:
============================
Search 'hello': FOUND
Search 'Hello': FOUND
Search 'WORLD': FOUND
Search 'world': FOUND

=== ALL TESTS COMPLETED ===
```

---

## 👥 Authors

[@rowidaEl](https://github.com/rowidaEl)

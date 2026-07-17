
# LLinstruct — Human Guide v1.0

## What is this?

A programming language for AI instructions. Attach `index.md` to any LLM, then write prompts in `instruct` blocks. The AI becomes a strict execution machine — no fluff, no guessing, no extra words.

---

## Quick Start

### 1. Attach the file
Upload `index.md` to ChatGPT, Claude, Gemini, or any LLM that supports file attachments.

### 2. Wait for ready signal
The AI will respond:
```
Я все понял. Жду задания.
```
(or in your language)

### 3. Write your instruction

````
```instruct
@language: ru
@code-language: python

/define|numbers|[1, 2, 3, 4, 5]
/filter|numbers|>2
/sort|result|DESC
```
````

### 4. Get clean output
```
[5, 4, 3]
```

No "Here is your result...", no explanations. Just data.

---

## Syntax Basics

### Parameters (set in prompt, after attach)

| Parameter | Values | Default | Description |
|-----------|--------|---------|-------------|
| `@language` | `ru`, `en`, `auto`, any | `ru` | Response language |
| `@code-language` | `python`, `js`, `auto`, any | `auto` | Code block language |
| `@temperature` | `0.0` – `1.0` | `0.0` | Creativity level |
| `@max_tokens` | number or `auto` | `500` | Max response length |
| `@format` | `raw`, `json`, `yaml`, `csv`, `table`, `list` | `raw` | Output format |
| `@style` | `minimal`, `verbose`, `step_by_step` | `minimal` | Explanation style |

### Set params in prompt

````
```instruct
@language: en
@code-language: javascript
@format: json

/define|user|{"name": "Ivan", "age": 30}
/recall|user
```
````

Output:
```json
{"name": "Ivan", "age": 30}
```

---

## Command Reference

### Flow Control

| Command | Syntax | Description |
|---------|--------|-------------|
| `/refuse` | `/refuse\|reason` | Stop and deny request |
| `/know` | `/know\|context` | Stop and admit lack of knowledge |
| `/clarify` | `/clarify\|question` | Stop and ask for clarification |
| `/exec` | `/exec\|block_name` | Execute named block |
| `/return` | `/return\|value` | Return value and stop |

### Variables

| Command | Syntax | Description |
|---------|--------|-------------|
| `/define` | `/define\|key\|value` | Store variable |
| `/recall` | `/recall\|key` | Get variable |
| `/forget` | `/forget\|key` | Delete variable |

### Stack Operations

| Command | Syntax | Description |
|---------|--------|-------------|
| `/stack` | `/stack\|value` | Push to stack |
| `/pop` | `/pop` | Pop and output top |
| `/dup` | `/dup` | Duplicate top |
| `/swap` | `/swap` | Swap top two |
| `/drop` | `/drop` | Remove top |

### Conditionals & Loops

| Command | Syntax | Description |
|---------|--------|-------------|
| `/if` | `/if\|a==b\|true_block\|false_block` | Conditional jump |
| `/goto` | `/goto\|block_name` | Unconditional jump |
| `/loop` | `/loop\|times\|block_name` | Repeat block N times |
| `/break` | `/break` | Exit current loop |
| `/continue` | `/continue` | Skip to next iteration |

### Math

| Command | Syntax | Description |
|---------|--------|-------------|
| `/math` | `/math\|+\|1\|2` | Add: 1 + 2 |
| `/math` | `/math\|*\|3\|4` | Multiply: 3 × 4 |
| `/math` | `/math\|sqrt\|16` | Square root |
| `/math` | `/math\|random\|1\|100` | Random number |
| `/math` | `/math\|avg\|1,2,3,4,5` | Average |

### Array/List

| Command | Syntax | Description |
|---------|--------|-------------|
| `/filter` | `/filter\|[1,2,3]\|>1` | Filter values |
| `/map` | `/map\|[1,2,3]\|*2` | Transform each |
| `/sort` | `/sort\|[3,1,2]\|ASC` | Sort array |
| `/slice` | `/slice\|[1,2,3,4]\|1\|3` | Get sub-array |
| `/join` | `/join\|[a,b,c]\|,` | Join to string |
| `/reduce` | `/reduce\|[1,2,3]\|+\|0` | Sum: 6 |

### String

| Command | Syntax | Description |
|---------|--------|-------------|
| `/split` | `/split\|hello world\| ` | Split to array |
| `/replace` | `/replace\|hello\|l\|L\|all` | Replace text |
| `/search` | `/search\|hello world\|world` | Find substring |
| `/format` | `/format\|uppercase\|hello` | HELLO |

### Data Transform

| Command | Syntax | Description |
|---------|--------|-------------|
| `/encode` | `/encode\|hello\|base64` | aGVsbG8= |
| `/decode` | `/decode\|aGVsbG8=\|base64` | hello |
| `/hash` | `/hash\|hello\|sha256` | Hash string |

### Validation

| Command | Syntax | Description |
|---------|--------|-------------|
| `/schema` | `/schema\|name:string:true\|age:int:false:0` | Define schema |
| `/validate` | `/validate\|data\|schema_name` | Validate data |
| `/assert` | `/assert\|x>0` | Assert condition |

### Debug

| Command | Syntax | Description |
|---------|--------|-------------|
| `/debug` | `/debug\|on` | Toggle debug |
| `/inspect` | `/inspect\|vars` | Show variables |
| `/inspect` | `/inspect\|stack` | Show stack |
| `/log` | `/log\|info\|message` | Internal log |

---

## Block System

Define reusable blocks:

````
```instruct
#BLOCK add_numbers
/stack|1
/stack|2
/math|+|pop|pop
/return

#BLOCK main
/exec|add_numbers
```
````

---

## Examples

### Example 1: Filter & Sort Data

````
```instruct
@language: en
@format: table

/define|users|[
  {"name":"Ivan","age":30},
  {"name":"Petr","age":25},
  {"name":"Anna","age":35}
]
/filter|users|age>25
/sort|result|age|ASC
```
````

### Example 2: Simple Calculator

````
```instruct
@language: ru

/stack|10
/stack|5
/math|+|pop|pop
```
````
Output: `15`

### Example 3: String Processing

````
```instruct
/define|text|"  Hello, World!  "
/transform|text|trim|lowercase
/replace|text|world|universe
```
````
Output: `hello, universe!`

### Example 4: JSON API Simulation

````
```instruct
@format: json

/define|response|{
  "status": "ok",
  "data": [1, 2, 3],
  "count": 3
}
/recall|response
```
````

### Example 5: Loop

````
```instruct
#BLOCK count_up
/define|counter|0

#BLOCK loop_body
/math|+|counter|1
/define|counter|result
/recall|counter

/loop|5|loop_body
```
````
Output: `1 2 3 4 5`

### Example 6: If/Else

````
```instruct
/define|age|17
/if|age>=18|adult|minor

#BLOCK adult
/return|"Access granted"

#BLOCK minor
/return|"Access denied"
```
````
Output: `Access denied`

---

## Error Messages

| Code | Meaning |
|------|---------|
| E001 | Syntax error |
| E002 | Type error |
| E003 | Runtime error |
| E009 | Unknown command |
| E010 | Missing parameter |
| E011 | Permission denied |
| E012 | Network error |

Example error:
```
ERROR: [E009] Unknown command at line 3
```

---

## Pro Tips

1. **Keep it minimal** — every token counts
2. **Use blocks** for reusable logic
3. **Chain commands** with pipes
4. **Set @temperature: 0.0** for deterministic output
5. **Use @format: json** for machine-readable responses
6. **Don't ask** — tell what to do
7. **No please/thanks** — it's machine language

---

## Full Pipeline Example

Attach `index.md`, then:

````
```instruct
@language: en
@code-language: python
@format: raw

# Get data
/define|nums|[5, 3, 8, 1, 9, 2, 7]

# Filter > 4
/filter|nums|>4

# Sort descending
/sort|result|DESC

# Double each
/map|result|*2

# Output
```
````

Output:
```
[18, 16, 14, 10]
```

That's it. No fluff. Pure execution.

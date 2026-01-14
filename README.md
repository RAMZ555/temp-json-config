# Comparison Ignore Configuration

The configuration works in **three modes**:

1. **Mode 1 – Global Ignore (applies to all test cases)**
2. **Mode 2 – Test Case Ignore + Global (merge behavior)**
3. **Mode 3 – Test Case Ignore Only (override behavior)**

The behavior is controlled using the flag:

```json
"inheritGlobalIgnore": true | false
```

---

## Mode 1: Global Ignore (Applied to All Test Cases)

### Configuration

```json
"comparisonConfig": {
  "ignoreElements": ["MsgId", "CreDtTm", "Dt", "EndToEndId"]
}
```

### Behavior

* Defined at the **root level** of the test definition
* Automatically applies to **every test case**
* No test-case override is required

### Effective Ignore Elements

```
MsgId, CreDtTm, Dt, EndToEndId
```


## Mode 2: Test Case Specific Ignore (Merged with Global)

### Configuration

```json
"success": [
  {
    "description": "Test case 1",
    "inputFileLoc": "/path/to/input.txt",
    "expectedOutputFile": "/path/to/expected.xml",
    "comparisonConfig": {
      "ignoreElements": ["BatchId", "TxnRef"],
      "inheritGlobalIgnore": true
    }
  }
]
```

### Behavior

* Test case defines its **own ignore elements**
* `inheritGlobalIgnore: true` means:

  * Global ignore rules are **included**
  * Test case ignore rules are **added**

### Effective Ignore Elements (for this test case only)

```
MsgId, CreDtTm, Dt, EndToEndId, BatchId, TxnRef
```

### Key Rule

```
ignoreElements = globalIgnore + testCaseIgnore
```

## Mode 3: Test Case Ignore Only (Global Ignored)

### Configuration

```json
{
  "description": "Special test - ignore different fields",
  "inputFileLoc": "/path/to/input.txt",
  "expectedOutputFile": "/path/to/expected.xml",
  "comparisonConfig": {
    "ignoreElements": ["SpecialId", "CustomField"],
    "inheritGlobalIgnore": false
  }
}
```

### Behavior

* Test case defines a **completely independent ignore list**
* `inheritGlobalIgnore: false` means:

  * Global ignore configuration is **NOT applied**

### Effective Ignore Elements

```
SpecialId, CustomField
```

### Key Rule

```
ignoreElements = testCaseIgnore (global ignored)
```



## Summary Table

| Mode   | Scope Used         | inheritGlobalIgnore | Effective Ignore Rules |
| ------ | ------------------ | ------------------- | ---------------------- |
| Mode 1 | Global             | N/A                 | Global only            |
| Mode 2 | Global + Test Case | true                | Global + Test Case     |
| Mode 3 | Test Case Only     | false               | Test Case only         |



      

  

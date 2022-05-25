---
title: "JSON в PowerShell"
keywords: [JSON, PowerShell]
---

Массив в PowerShell:

```powershell
$body = @(
    'value1', 'value2', 'value3'
)
ConvertTo-Json $body
```

```json
[
    "value1",
    "value2",
    "value3"
]
```

Объект в PowerShell:

```powershell
$body = @{
    key1 = 'value1'
    key2 = 'value2'
    key3 = 'value3'
}
ConvertTo-Json $body
```

```json
{
    "key3":  "value3",
    "key1":  "value1",
    "key2":  "value2"
}
```

Объект с массивом в PowerShell:

```powershell
$body = @{
    key1 = 'value1'
    key2 = 'value2'
    key3 = 'value3'
    key4 = @(
        'value1', 'value2', 'value3'
    )
    key5 = 'value5'
    key6 = @{
        key7 = 'value7'
        key8 = 'value9'
        key9 = 'value8'
    }
}
ConvertTo-Json $body
```

```json
{
    "key5":  "value5",
    "key4":  [
                 "value1",
                 "value2",
                 "value3"
             ],
    "key1":  "value1",
    "key3":  "value3",
    "key6":  {
                 "key9":  "value8",
                 "key8":  "value9",
                 "key7":  "value7"
             },
    "key2":  "value2"
}
```

```powershell
$listValue = New-Object System.Collections.ArrayList
$listValue.Add(@{
        key1 = 'value1'
        key2 = 'value2'
        key3 = 'value3'
    })
$listValue.Add(@{
        key1 = 'value1'
        key2 = 'value2'
        key3 = 'value3'
    })
$bodyArray = @()
$bodyObject = @{}
$bodyArray += $listValue
$bodyObject['array'] = $bodyArray
$bodyObject | ConvertTo-Json
```

```json
{
    "array":  [
                  {
                      "key3":  "value3",
                      "key1":  "value1",
                      "key2":  "value2"
                  },
                  {
                      "key3":  "value3",
                      "key1":  "value1",
                      "key2":  "value2"
                  }
              ]
}
```

## Ссылки

[[json_format]] Формат JSON

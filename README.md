# JSON Config Flattener

A single-file browser tool for converting between nested JSON and flat colon-separated key:value pairs — the format used by **.NET `IConfiguration`**, Kubernetes secrets/configmaps, and ArgoCD Helm value overrides.

## Live demo

Open `index.html` directly in your browser — no server or build step needed.

## What it does

| Direction | Input | Output |
|---|---|---|
| **Flatten →** | Nested JSON object | Flat `"Key:Sub:Key": "value"` pairs, sorted A–Z |
| **← Unflatten** | Flat key:value JSON | Reconstructed nested JSON with inferred types |

### Example

**Input (nested)**
```json
{
  "Tracing": {
    "EnableTracing": true,
    "Otlp": {
      "Endpoint": "http://localhost:55680"
    }
  },
  "RabbitMQ": {
    "Publisher": {
      "ExchangeName": "my.exchange"
    }
  }
}
```

**Output (flattened)**
```json
{
  "RabbitMQ:Publisher:ExchangeName": "my.exchange",
  "Tracing:EnableTracing": "true",
  "Tracing:Otlp:Endpoint": "http://localhost:55680"
}
```

## Features

- **Separator**: `:` (matches .NET `IConfiguration` path convention)
- **Arrays** are indexed: `WriteTo:0:Name`, `WriteTo:1:Name`, …
- **Flatten** coerces all values to strings
- **Unflatten** infers types back: `"true"` → `bool`, `"42"` → `int`, `"3.14"` → `float`, everything else → `string`
- **Sort**: flattened output is always sorted alphabetically
- **Swap** panels with ⇄ to quickly round-trip
- **Download** as `flattened.json`
- **Keyboard shortcuts**: `Ctrl+Enter` flatten, `Ctrl+Shift+Enter` unflatten

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Enter` | Flatten (left → right) |
| `Ctrl+Shift+Enter` | Unflatten (right → left) |

## Usage

```
git clone https://github.com/your-username/json-flattener
open index.html
```

Or just download `index.html` and open it — it's self-contained.

## Use cases

- Preparing `.NET` `appsettings.json` overrides for Kubernetes `ConfigMap`s
- Generating ArgoCD Helm `--set` flags from nested config
- Comparing flat environment variable configs with their nested source
- Debugging environment-specific config differences in CI/CD pipelines

## License

MIT

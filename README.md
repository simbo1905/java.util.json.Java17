# java.util.json.Java17

A backport of `java.util.json` to JDK17 to work on Android.

This is a minimal backport of the core JSON API from [simbo1905/java.util.json.Java21](https://github.com/simbo1905/java.util.json.Java21) adapted for Java 17 and Android compatibility.

## Overview

This library provides a type-safe, immutable JSON API that works on Java 17+ and Android. It includes:

- **Core JSON API**: Sealed interfaces for JSON types (JsonObject, JsonArray, JsonString, JsonNumber, JsonBoolean, JsonNull)
- **Parser**: RFC 8259 compliant JSON parser
- **Type conversion**: Convert between typed JSON values and untyped Java objects
- **Display formatting**: Pretty-print JSON with configurable indentation

## Features

- ✅ **Java 17 Compatible**: Uses preview features (pattern matching in switch) available in Java 17
- ✅ **Android Ready**: No Java 21-specific APIs, works on Android
- ✅ **Type-Safe**: Sealed interfaces provide exhaustive pattern matching
- ✅ **Immutable**: All JSON values are immutable
- ✅ **RFC 8259 Compliant**: Strictly follows JSON specification
- ✅ **Full Test Suite**: Comprehensive tests ported from Java21 version

## Requirements

- Java 17 or later
- Gradle 8.5+ (included via wrapper)
- `--enable-preview` flag (automatically configured)

## Building

```bash
./gradlew build
```

## Running Tests

```bash
./gradlew test
```

## Usage Examples

### Parsing JSON

```java
import jdk.sandbox.java.util.json.*;

// Parse JSON string
JsonValue value = Json.parse("""
    {
        "name": "Alice",
        "age": 30,
        "active": true
    }
    """);

// Access as typed object
JsonObject obj = (JsonObject) value;
String name = ((JsonString) obj.members().get("name")).value();
```

### Building JSON

```java
// Create JSON programmatically
JsonObject user = JsonObject.of(Map.of(
    "name", JsonString.of("Bob"),
    "age", JsonNumber.of(25),
    "roles", JsonArray.of(List.of(
        JsonString.of("admin"),
        JsonString.of("user")
    ))
));
```

### Type Conversion

```java
// From untyped Java objects
Map<String, Object> data = Map.of(
    "name", "Charlie",
    "score", 95
);
JsonValue json = Json.fromUntyped(data);

// To untyped Java objects
Object obj = Json.toUntyped(json);
```

### Pretty Printing

```java
JsonObject data = JsonObject.of(Map.of(
    "name", JsonString.of("Alice"),
    "scores", JsonArray.of(List.of(
        JsonNumber.of(85),
        JsonNumber.of(90)
    ))
));

String formatted = Json.toDisplayString(data, 2);
```

## Project Scope

This backport includes ONLY:
- Core JSON API from `json-java21` module
- Full test suite
- **NO** JDT (JSON Type Definition) implementation
- **NO** API tracker
- **NO** compatibility suite

## Differences from Java21 Version

### API Changes
- `LinkedHashMap.newLinkedHashMap(int)` → `new LinkedHashMap<>(int)`
- `List.getFirst()` → `List.get(0)`
- Removed redundant `instanceof JsonValue` pattern matches

### Build Configuration
- Changed from Maven to Gradle
- Enabled preview features for pattern matching in switch
- Configured for Java 17 source/target compatibility

## License

Licensed under the GNU General Public License version 2 with Classpath exception, matching the upstream OpenJDK sources.

## Upstream

This code is derived from:
- **Upstream**: [OpenJDK jdk-sandbox](https://github.com/openjdk/jdk-sandbox) "json" branch
- **Java21 Backport**: [simbo1905/java.util.json.Java21](https://github.com/simbo1905/java.util.json.Java21)
- **Commit**: a8e7de8b49e4e4178eb53c94ead2fa2846c30635 (2025-08-14)

## Status

⚠️ **Experimental**: This is an unstable API not intended for production use. The upstream API is still evolving.

## Credits

- **Original Design**: OpenJDK JSON sandbox team
- **Java21 Backport**: Simon Massey (@simbo1905)
- **Java17 Adaptation**: This repository


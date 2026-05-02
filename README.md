# cpfhub: Dart/Flutter SDK for CPFHub.io

🇺🇸 **English** | [🇧🇷 Português](#português)

**Official Dart/Flutter SDK for [CPFHub.io](https://cpfhub.io) — Brazilian CPF Lookup API**

[![pub.dev](https://img.shields.io/pub/v/cpfhub)](https://pub.dev/packages/cpfhub)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

---

## What is CPFHub.io?

CPFHub.io is a REST API that returns name, gender, and date of birth from any Brazilian CPF number — in ~300ms, with 99.9% uptime and full LGPD compliance.

**10M+ CPFs queried · 1,300+ active companies · 99.9% uptime**

---

## Installation

```yaml
# pubspec.yaml
dependencies:
  cpfhub: ^1.0.0
```

---

## Quick Start

```dart
import 'package:cpfhub/cpfhub.dart';

void main() async {
  final client = CPFHubClient(apiKey: 'YOUR_API_KEY');
  final result = await client.lookup('00000000000');
  print(result.name);      // "Fulano de Tal"
  print(result.gender);    // "M"
  print(result.birthDate); // "15/06/1990"
}
```

Get your free API key at [app.cpfhub.io](https://app.cpfhub.io) — no credit card required.

---

## curl Example

```bash
curl -X GET "https://api.cpfhub.io/cpf/12345678909" \\
  -H "x-api-key: YOUR_API_KEY"
```

**Response:**

```json
{
  "success": true,
  "data": {
    "cpf": "12345678909",
    "name": "Fulano de Tal",
    "nameUpper": "FULANO DE TAL",
    "gender": "M",
    "birthDate": "15/06/1990",
    "day": 15,
    "month": 6,
    "year": 1990
  }
}
```

---

## API Reference

### `CPFHubClient({required String apiKey, Duration? timeout})`

### `client.lookup(String cpf) -> Future<CPFResult>`

Looks up a CPF and returns the associated identity data.

Accepts CPF with or without formatting (`000.000.000-00` or `00000000000`).

#### `CPFResult` fields

| Field | Type | Description |
|-------|------|-------------|
| `cpf` | `String` | CPF number (digits only) |
| `name` | `String` | Full name |
| `nameUpper` | `String` | Full name in uppercase |
| `gender` | `String` | `"M"` or `"F"` |
| `birthDate` | `String` | Date of birth — `"DD/MM/YYYY"` |
| `day` | `int` | Birth day |
| `month` | `int` | Birth month |
| `year` | `int` | Birth year |

---

## Error Handling

```dart
try {
  final result = await client.lookup('00000000000');
  print(result.name);
} on CPFHubException catch (e) {
  print('Error ${e.statusCode}: ${e.message}');
  // 400 — Invalid CPF format
  // 401 — Invalid or missing API key
  // 404 — CPF not found
  // 429 — Rate limit exceeded
  // 500 — Server error
  // 503 — Service temporarily unavailable
}
```

---

## Examples

### Real-world onboarding

```dart
Future<void> onboardUser(String cpf, String email) async {
  final client = CPFHubClient(apiKey: 'YOUR_API_KEY');
  final identity = await client.lookup(cpf);
  await createUser(cpf: cpf, name: identity.name, email: email, birthYear: identity.year);
}
```

---

## Rate Limits

| Plan | Limit |
|------|-------|
| Free | 1 request every 2 seconds · 50 requests/month |
| Pro | 1 request per second · 1,000 requests/month |
| Corporate | Custom |

---

## Plans & Pricing

| Plan | Price | Included | Extra |
|------|-------|----------|-------|
| **Free** | R$ 0/month | 50 lookups | — |
| **Pro** | R$ 149/month | 1,000 lookups | R$ 0,15/lookup |
| **Corporate** | Custom | Custom | Custom |

[View full pricing at cpfhub.io →](https://cpfhub.io#pricing)

---

## Requirements

- Dart 3.0+
- Flutter 3.0+ (for Flutter projects)

---

## Links

- [Documentation](https://cpfhub.io/documentacao)
- [pub.dev Package](https://pub.dev/packages/cpfhub)
- [Dashboard](https://app.cpfhub.io)
- [Status Page](https://app.cpfhub.io/status)
- [Pricing](https://cpfhub.io#pricing)
- [LGPD Compliance](https://cpfhub.io/lgpd)
- [OpenAPI Specification](https://github.com/cpfhub/cpfhub-openapi/blob/main/openapi.yaml)
- [MCP Server (AI Agents)](https://github.com/cpfhub/cpfhub-mcp)

---

## License

MIT © [CPFHub.io](https://cpfhub.io)

---

# Português

[🇺🇸 English](#cpfhub-dartflutter-sdk-for-cpfhubio) | 🇧🇷 **Português**

**SDK Dart/Flutter oficial para [CPFHub.io](https://cpfhub.io) — API de Consulta de CPF Brasileiro**

---

## O que é o CPFHub.io?

O CPFHub.io é uma API REST que retorna nome, gênero e data de nascimento de qualquer CPF brasileiro — em ~300ms, com 99,9% de uptime e total conformidade com a LGPD.

**10M+ CPFs consultados · 1.300+ empresas ativas · 99,9% uptime**

---

## Instalação

```yaml
# pubspec.yaml
dependencies:
  cpfhub: ^1.0.0
```

---

## Início Rápido

```dart
import 'package:cpfhub/cpfhub.dart';

void main() async {
  final client = CPFHubClient(apiKey: 'SUA_CHAVE_DE_API');
  final result = await client.lookup('00000000000');
  print(result.name);      // "Fulano de Tal"
  print(result.gender);    // "M"
  print(result.birthDate); // "15/06/1990"
}
```

Obtenha sua chave de API gratuita em [app.cpfhub.io](https://app.cpfhub.io) — sem cartão de crédito.

---

## Exemplo curl

```bash
curl -X GET "https://api.cpfhub.io/cpf/12345678909" \\
  -H "x-api-key: SUA_CHAVE_DE_API"
```

**Resposta:**

```json
{
  "success": true,
  "data": {
    "cpf": "12345678909",
    "name": "Fulano de Tal",
    "nameUpper": "FULANO DE TAL",
    "gender": "M",
    "birthDate": "15/06/1990",
    "day": 15,
    "month": 6,
    "year": 1990
  }
}
```

---

## Referência da API

### `CPFHubClient({required String apiKey, Duration? timeout})`

### `client.lookup(String cpf) -> Future<CPFResult>`

Consulta um CPF e retorna os dados de identidade associados.

Aceita CPF com ou sem formatação (`000.000.000-00` ou `00000000000`).

#### Campos de `CPFResult`

| Campo | Tipo | Descrição |
|-------|------|----------|
| `cpf` | `String` | CPF (apenas dígitos) |
| `name` | `String` | Nome completo |
| `nameUpper` | `String` | Nome completo em maiúsculas |
| `gender` | `String` | `"M"` ou `"F"` |
| `birthDate` | `String` | Data de nascimento — `"DD/MM/YYYY"` |
| `day` | `int` | Dia de nascimento |
| `month` | `int` | Mês de nascimento |
| `year` | `int` | Ano de nascimento |

---

## Tratamento de Erros

```dart
try {
  final result = await client.lookup('00000000000');
  print(result.name);
} on CPFHubException catch (e) {
  print('Erro ${e.statusCode}: ${e.message}');
  // 400 — Formato de CPF inválido
  // 401 — Chave de API inválida ou ausente
  // 404 — CPF não encontrado
  // 429 — Limite de requisições excedido
  // 500 — Erro no servidor
  // 503 — Serviço temporariamente indisponível
}
```

---

## Exemplos

### Onboarding real

```dart
Future<void> onboardUser(String cpf, String email) async {
  final client = CPFHubClient(apiKey: 'SUA_CHAVE_DE_API');
  final identity = await client.lookup(cpf);
  await createUser(cpf: cpf, name: identity.name, email: email, birthYear: identity.year);
}
```

---

## Limites de Requisição

| Plano | Limite |
|-------|--------|
| Gratuito | 1 requisição a cada 2 segundos · 50 requisições/mês |
| Pro | 1 requisição por segundo · 1.000 requisições/mês |
| Corporativo | Personalizado |

---

## Planos e Preços

| Plano | Preço | Incluído | Extra |
|-------|-------|----------|-------|
| **Gratuito** | R$ 0/mês | 50 consultas | — |
| **Pro** | R$ 149/mês | 1.000 consultas | R$ 0,15/consulta |
| **Corporativo** | Personalizado | Personalizado | Personalizado |

[Ver preços completos em cpfhub.io →](https://cpfhub.io#pricing)

---

## Requisitos

- Dart 3.0+
- Flutter 3.0+ (para projetos Flutter)

---

## Links

- [Documentação](https://cpfhub.io/documentacao)
- [Pacote pub.dev](https://pub.dev/packages/cpfhub)
- [Dashboard](https://app.cpfhub.io)
- [Página de Status](https://app.cpfhub.io/status)
- [Preços](https://cpfhub.io#pricing)
- [Conformidade LGPD](https://cpfhub.io/lgpd)
- [Especificação OpenAPI](https://github.com/cpfhub/cpfhub-openapi/blob/main/openapi.yaml)
- [Servidor MCP (Agentes de IA)](https://github.com/cpfhub/cpfhub-mcp)

---

## Licença

MIT © [CPFHub.io](https://cpfhub.io)

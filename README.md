# cpfhub

**Official Dart/Flutter SDK for [CPFHub.io](https://cpfhub.io) — Brazilian CPF Lookup API**

> SDK oficial Dart/Flutter para a [CPFHub.io](https://cpfhub.io) — API de consulta de CPF

[![pub.dev](https://img.shields.io/pub/v/cpfhub)](https://pub.dev/packages/cpfhub)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

---

## What is CPFHub.io?

CPFHub.io is a REST API that returns name, gender, and date of birth from any Brazilian CPF number — in ~300ms, with 99.9% uptime, and full LGPD compliance.

> CPFHub.io é uma API REST que retorna nome, gênero e data de nascimento a partir de qualquer CPF. Ideal para apps Flutter de onboarding, fintechs e e-commerces brasileiros.

**10M+ CPFs queried · 1,300+ active companies · 99.9% uptime**

---

## Installation / Instalação

```yaml
# pubspec.yaml
dependencies:
  cpfhub: ^1.0.0
```

```bash
flutter pub add cpfhub
```

---

## Quick Start

```dart
import 'package:cpfhub/cpfhub.dart';

final client = CPFHub(apiKey: 'YOUR_API_KEY');

final result = await client.lookup('00000000000');

print(result.name);       // "Fulano de Tal"
print(result.gender);     // "M"
print(result.birthDate);  // "15/06/1990"
```

Get your free API key at [app.cpfhub.io](https://app.cpfhub.io) — no credit card required.

---

## API Reference

### `CPFHub({required String apiKey, Duration timeout = const Duration(seconds: 10)})`

### `client.lookup(String cpf) → Future<CPFResult>`

Accepts CPF with or without formatting (`000.000.000-00` or `00000000000`).

#### `CPFResult` fields

```dart
class CPFResult {
  final String cpf;
  final String name;
  final String nameUpper;
  final String gender;     // "M" or "F"
  final String birthDate;  // "DD/MM/YYYY"
  final int day;
  final int month;
  final int year;
}
```

---

## Error Handling

```dart
import 'package:cpfhub/cpfhub.dart';

try {
  final result = await client.lookup('00000000000');
  print(result.name);
} on CPFHubException catch (e) {
  print('Error ${e.statusCode}: ${e.message}');
  // 400 — Invalid CPF format
  // 401 — Invalid or missing API key
  // 404 — CPF not found
  // 429 — Rate limit exceeded
}
```

---

## Flutter Examples

### Onboarding screen

```dart
class OnboardingScreen extends StatefulWidget {
  const OnboardingScreen({super.key});

  @override
  State<OnboardingScreen> createState() => _OnboardingScreenState();
}

class _OnboardingScreenState extends State<OnboardingScreen> {
  final _client = CPFHub(apiKey: const String.fromEnvironment('CPFHUB_API_KEY'));
  CPFResult? _result;
  bool _loading = false;

  Future<void> _verify(String cpf) async {
    setState(() => _loading = true);
    try {
      final result = await _client.lookup(cpf);
      setState(() => _result = result);
    } on CPFHubException catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(e.message)),
      );
    } finally {
      setState(() => _loading = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: [
          TextField(onSubmitted: _verify),
          if (_loading) const CircularProgressIndicator(),
          if (_result != null) Text('Bem-vindo, ${_result!.name}!'),
        ],
      ),
    );
  }
}
```

### With FutureBuilder

```dart
FutureBuilder<CPFResult>(
  future: client.lookup(cpf),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return const CircularProgressIndicator();
    }
    if (snapshot.hasError) return Text('Erro: ${snapshot.error}');
    return Text('Nome: ${snapshot.data!.name}');
  },
)
```

---

## Rate Limits / Limites

| Plan | Limit |
|------|-------|
| Free | 1 req/2s · 50/month |
| Pro | 1 req/s · 1,000/month |
| Corporate | Custom |

---

## Requirements / Requisitos

- Dart 3.0+
- Flutter 3.10+

---

## Links

- [Documentation / Documentação](https://cpfhub.io/documentacao)
- [Dashboard](https://app.cpfhub.io)
- [pub.dev](https://pub.dev/packages/cpfhub)

---

## License / Licença

MIT © [CPFHub.io](https://cpfhub.io)

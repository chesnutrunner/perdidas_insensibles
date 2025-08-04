# Calculadora de Pérdidas Insensibles

Una aplicación móvil desarrollada en Flutter para calcular las pérdidas insensibles en contextos de enfermería.

## Características

- **Tema oscuro** con colores específicos para ambientes hospitalarios
- **Tres controles deslizantes** para configurar:
  - Peso: 25-150 kg (valor inicial: 70 kg)
  - Temperatura: 33-41°C (valor inicial: 36.5°C)
  - Horas de estancia: 1-24 h (valor inicial: 7 h)
- **Cálculo automático** usando fórmula médica con constantes de temperatura
- **Botones de acción**:
  - Calcular (verde #00C853)
  - Reset (gris #424242)
- **Diseño responsive** optimizado para dispositivos móviles
- **Orientación forzada** en vertical para uso clínico

## Fórmula de Cálculo

```
Pérdidas Insensibles = Peso (kg) × Horas (h) × ConstanteTemp
```

### Constante de Temperatura:
- **Normal (36.5-37.5°C)**: 0.5
- **Fiebre (>37.5°C)**: 0.5 + ((Temp - 37.5) × 0.1)
- **Hipotermia (<36.5°C)**: 0.5 - ((36.5 - Temp) × 0.1)

## Compilación para Android

### Requisitos previos:
- Flutter SDK (>=3.0.0)
- Android SDK
- Java 17 o superior

### Pasos para compilar APK:

1. **Navegar al directorio del proyecto:**
   ```bash
   cd flutter_app
   ```

2. **Instalar dependencias:**
   ```bash
   flutter pub get
   ```

3. **Compilar APK de release:**
   ```bash
   flutter build apk --release
   ```

4. **El archivo APK se generará en:**
   ```
   flutter_app/build/app/outputs/flutter-apk/app-release.apk
   ```

### Compilación en servicios en la nube:

#### Codemagic:
1. Conectar el repositorio
2. Configurar workflow para Flutter
3. El APK se generará automáticamente

#### GitHub Actions:
```yaml
name: Build Flutter APK
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: subosito/flutter-action@v2
      with:
        flutter-version: '3.19.0'
    - run: cd flutter_app && flutter pub get
    - run: cd flutter_app && flutter build apk --release
```

## Estructura del Proyecto

```
flutter_app/
├── lib/
│   └── main.dart              # Código principal de la aplicación
├── android/                   # Configuración Android
│   ├── app/
│   │   ├── build.gradle       # Configuración de compilación
│   │   └── src/main/
│   │       ├── AndroidManifest.xml
│   │       └── kotlin/        # Código nativo Android
│   └── build.gradle          # Configuración global
├── pubspec.yaml              # Dependencias y metadatos
└── README.md                 # Este archivo
```

## Uso Clínico

1. **Ajustar parámetros** usando los controles deslizantes
2. **Presionar "Calcular"** para obtener el resultado en ml
3. **Usar "Reset"** para volver a valores por defecto
4. **Interpretar resultado** considerando valores recomendados (36.5-37.5°C)

## Validación

La aplicación valida automáticamente que:
- Los valores estén dentro de rangos clínicos seguros
- La temperatura tenga incrementos de 0.1°C
- El resultado se muestre con precisión de 2 decimales

## Personalización

Para modificar los rangos o valores por defecto, editar las constantes en `main.dart`:
- `peso = 70.0` (línea 23)
- `temperatura = 36.5` (línea 24)  
- `horas = 7.0` (línea 25)

## Soporte

Esta aplicación está diseñada específicamente para profesionales de enfermería y debe usarse como herramienta de apoyo, no como sustituto del juicio clínico profesional.
# 📱 Instrucciones de Compilación - APK Android

## 🎯 Objetivo
Generar un archivo APK instalable para Android de la aplicación "Calculadora de Pérdidas Insensibles".

## ⚡ Compilación Rápida

### Opción 1: Usando Codemagic (Recomendado)
1. **Subir a GitHub/GitLab**:
   - Crear repositorio nuevo
   - Subir toda la carpeta `flutter_app/`

2. **Configurar Codemagic**:
   - Ir a [codemagic.io](https://codemagic.io)
   - Conectar repositorio
   - Seleccionar "Flutter App"
   - **Configuración automática**: Codemagic detectará el proyecto Flutter

3. **Iniciar build**:
   - Click en "Start new build"
   - Seleccionar "Android" como target
   - El APK se generará en 5-10 minutos

### Opción 2: GitHub Actions (Automatizado)
Crear archivo `.github/workflows/build.yml` en la raíz del proyecto:

```yaml
name: Build APK
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: subosito/flutter-action@v2
      with:
        flutter-version: '3.19.0'
        channel: 'stable'
    
    - name: Get dependencies
      run: |
        cd flutter_app
        flutter pub get
    
    - name: Build APK
      run: |
        cd flutter_app
        flutter build apk --release
    
    - name: Upload APK
      uses: actions/upload-artifact@v4
      with:
        name: release-apk
        path: flutter_app/build/app/outputs/flutter-apk/app-release.apk
```

## 🔧 Compilación Local

### Prerrequisitos:
```bash
# 1. Instalar Flutter
git clone https://github.com/flutter/flutter.git -b stable
export PATH="$PATH:`pwd`/flutter/bin"

# 2. Verificar instalación
flutter doctor

# 3. Instalar Android SDK
# Descargar Android Studio o usar command-line tools
```

### Comandos de compilación:
```bash
# 1. Navegar al proyecto
cd flutter_app

# 2. Instalar dependencias
flutter pub get

# 3. Verificar dispositivos/emuladores
flutter devices

# 4. Compilar APK de release
flutter build apk --release

# 5. Ubicación del APK generado:
# flutter_app/build/app/outputs/flutter-apk/app-release.apk
```

## 📋 Verificaciones Previas

### Checklist antes de compilar:
- [ ] `pubspec.yaml` tiene nombre correcto: `perdidas_insensibles_calculadora`
- [ ] `AndroidManifest.xml` configurado correctamente
- [ ] Permisos de Android definidos
- [ ] Orientación forzada a portrait
- [ ] Ícono de aplicación presente

### Testear la aplicación:
```bash
# Ejecutar en modo debug
cd flutter_app
flutter run

# Ejecutar tests (si los hay)
flutter test

# Verificar análisis de código
flutter analyze
```

## 🚀 Servicios de Compilación en la Nube

### 1. **Codemagic** (Más fácil)
- ✅ Configuración automática
- ✅ Free tier disponible
- ✅ Integración con Git
- ⏱️ 5-10 minutos

### 2. **GitHub Actions** (Gratis)
- ✅ Completamente gratis
- ✅ Integración nativa con GitHub
- ⏱️ 8-15 minutos

### 3. **Bitrise**
- ✅ Especializado en móviles
- ✅ Free tier para proyectos pequeños
- ⏱️ 10-20 minutos

### 4. **Azure DevOps**
- ✅ 1800 minutos gratis/mes
- ✅ Integración con Microsoft
- ⏱️ 8-12 minutos

## 🐛 Solución de Problemas

### Error: "Flutter SDK not found"
```bash
export PATH="$PATH:/path/to/flutter/bin"
flutter doctor
```

### Error: "Android SDK not found"
```bash
# Instalar Android Studio
# O configurar ANDROID_HOME manualmente
export ANDROID_HOME=/path/to/android/sdk
```

### Error: "No connected devices"
```bash
# Para emulador
flutter emulators --launch <emulator_id>

# Para compilar APK (no necesita dispositivo)
flutter build apk --release
```

### APK muy grande (>50MB)
```bash
# Compilar con optimizaciones
flutter build apk --release --shrink

# O generar App Bundle (recomendado para Play Store)
flutter build appbundle --release
```

## 📦 Archivos Finales

Después de la compilación exitosa, tendrás:

1. **APK de Release**: `app-release.apk` (~15-25 MB)
2. **APK de Debug**: `app-debug.apk` (~35-50 MB) 
3. **App Bundle**: `app-release.aab` (para Play Store)

## 🔒 Firma Digital (Opcional)

Para distribución en Play Store:
```bash
# Generar keystore
keytool -genkey -v -keystore android-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key

# Configurar en android/key.properties
# Compilar con firma
flutter build apk --release
```

## ✅ Instalación en Dispositivo

```bash
# Habilitar "Fuentes desconocidas" en Android
# Transferir APK al dispositivo
# Instalar desde explorador de archivos
```

---

**💡 Tip**: Para desarrollo rápido, usar Codemagic. Para proyectos empresariales, usar GitHub Actions o Azure DevOps.
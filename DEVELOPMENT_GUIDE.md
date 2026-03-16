# دليل تطوير وتحديث Zatsu

## نظرة عامة

هذا الدليل يشرح كيفية:
1. تطوير وتعديل التطبيق
2. تحديث مصادر المانجا (zatsu-parsers)
3. اختبار التعديلات بسرعة

---

## الجزء الأول: هيكل المشروع

### المشروعين:
- **zatsu**: تطبيق قراءة المانجا (https://github.com/izukuX2/zatsu)
- **zatsu-parsers**: مكتبة المصادر (https://github.com/izukuX2/zatsu-parsers)

---

## الجزء الثاني: ربط zatsu بـ zatsu-parsers

### الطريقة 1: JitPack (للإصدارات الرسمية)

```toml
# في gradle/libs.versions.toml
parsers = "v1.0"  # أو commit hash
```

```kotlin
# في app/build.gradle
implementation("com.github.izukuX2:zatsu-parsers:$parsersVersion")
```

**المميزات:**
- ✅ سهل الإعداد
- ✅ مناسب للإصدارات الرسمية

**العيوب:**
- ❌ يتطلب رفع على GitHub لكل تعديل
- ❌ بطيء للاختبار

---

### الطريقة 2: Local Module (للاختبار السريع) ⚡

#### الخطوة 1: نسخ مجلد zatsu-parsers

```
zatsu/
├── app/
├── gradle/
└── zatsu-parsers/          # ← نسخ المجلد هنا
    ├── src/
    ├── build.gradle.kts
    └── ...
```

#### الخطوة 2: تعديل settings.gradle

```groovy
// في ملف settings.gradle الرئيسي
rootProject.name = "Zatsu"

include ':app'
include ':zatsu-parsers'    // ← أضف هذا
```

#### الخطوة 3: تعديل app/build.gradle

```groovy
// استبدل:
implementation("com.github.izukuX2:zatsu-parsers:$parsersVersion") {
    exclude group: 'org.json', module: 'json'
}

// بـ:
implementation(project(':zatsu-parsers')) {
    exclude group: 'org.json', module: 'json'
}
```

#### الخطوة 4: البناء والاختبار

```bash
./gradlew assembleDebug
```

**المميزات:**
- ✅ تعديل فوري بدون رفع
- ✅ اختبار سريع
- ✅ بدون اعتماد على الإنترنت

**العيوب:**
- ❌ يتطلب نسخ المجلد

---

## Parte Tres: flujo de trabajo

### Para pruebas rápidas (Local Module):

```
1. Modificar → zatsu-parsers/src/...
2. Construir → ./gradlew assembleDebug  
3. Probar → APK en app/build/outputs/
4. Repetir
```

### Para publicación oficial (JitPack):

```
1. Modificar → zatsu-parsers/src/...
2. Commit → git add . && git commit -m "fix: ..."
3. Push → git push
4. Tag → git tag v1.1 && git push --tags
5. Actualizar → parsers = "v1.1" en zatsu
6. Construir → ./gradlew assembleDebug
7. Release → GitHub Release
```

---

## Parte Cuatro: actualizaciones de zatsu-parsers

### Estructura de fuentes:

```
zatsu-parsers/src/main/kotlin/org/koitharu/kotatsu/parsers/
├── model/
│   └── MangaParserSource.kt      # 定义 fuentes
├── source/
│   ├── arabic/
│   │   ├── Manga4u.kt
│   │   ├── KokoroManga.kt
│   │   └── ...
│   └── ...
└── ...
```

### Agregar nueva fuente árabe:

1. **Crear archivo de parser** en `source/arabic/`
2. **Registrar en** `MangaParserSource.kt`
3. **Probar** con Local Module
4. **Publicar** cuando funcione

---

## Parte Cinco: Solución de problemas

### Error: "Cannot find module"
```bash
# Limpiar y reconstruir
./gradlew clean
./gradlew assembleDebug
```

### Error: "Version not found"
- Para JitPack: verificar Release en GitHub
- Para Local: verificar include en settings.gradle

### Error: "Build failed"
```bash
# Sincronizar gradle
./gradlew --refresh-dependencies
```

---

## Parte Seis: comandos útiles

```bash
# Construir debug APK
./gradlew assembleDebug

# Construir release APK
./gradlew assembleRelease

# Solo compilar
./gradlew compileDebugKotlin

# Limpiar
./gradlew clean

# Ver tareas disponibles
./gradlew tasks
```

---

## ملخص

| الطريقة | скорость |適用 |
|---------|---------|------|
| Local Module | ⚡ سريعة | اختبار |
| JitPack | 🔄 بطيئة | إصدار |

---

## معلومات إضافية

- **Gradle:** `./gradlew`
- **Kotlin:** يحتاج JDK 17+
- **Android SDK:** API 36

---

*Actualizado: Marzo 2026*
*Versión: 9.6.6*

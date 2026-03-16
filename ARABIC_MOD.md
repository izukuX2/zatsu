# Zatsu Arabic-Only Modifications

## Changes Made

### 1. WelcomeViewModel.kt
- Removed HENTAI content type from available options
- Changed default language selection from system locale to Arabic ("ar") only
- Removed auto-detection of system language

### 2. SourcesCatalogViewModel.kt  
- Removed HENTAI content type from the filter list
- Now always filters out HENTAI regardless of settings

### 3. SourcesManageFragment.kt
- Hidden NSFW toggle option from the sources management menu

### 4. AppSettings.kt
- Forced `isNsfwContentDisabled` to always return `true`
- Setting the value now always saves `true` (cannot be changed)

### 5. WelcomeSheet.kt
- Hidden language selection chips (locales section) completely

### 6. SourcesCatalogActivity.kt
- Removed language filter chip from sources catalog
- Forced Arabic locale in filter (hardcoded "ar")

### 7. MenuProviders (Bug Fix)
- Added `onPrepareMenu` override to multiple MenuProvider classes that were missing it

### 8. AppSettings.kt (Additional)
- Forced `isAllSourcesEnabled` to always return `false` (only Arabic sources)

### 9. app/build.gradle
- Changed parser dependency from `com.github.clquwu:kotatsu-parsers-redo` to `com.github.izukuX2:zatsu-parsers`

### 10. DEVELOPMENT_GUIDE.md (New)
- Added comprehensive development guide for testing and updating

---

## Development Workflow

### Quick Testing (Local Module):
1. Copy `zatsu-parsers` folder to project root
2. Uncomment `include ':zatsu-parsers'` in settings.gradle
3. Uncomment `implementation(project(':zatsu-parsers'))` in app/build.gradle
4. Run `./gradlew assembleDebug`
5. Test immediately!

### Production Release (JitPack):
1. Make changes in zatsu-parsers
2. Create GitHub Release with tag
3. Update `parsers = "v1.x.x"` in gradle/libs.versions.toml
4. Build and release APK

See `DEVELOPMENT_GUIDE.md` for full details.

### To allow other languages:
1. `WelcomeViewModel.kt`: Change `Locale("ar")` to include other locales or add back system locale detection
2. `WelcomeSheet.kt`: Change `binding.chipsLocales.isGone = true` to `false` to show language selection
3. `SourcesCatalogActivity.kt`: Restore the locale filter chip and remove the hardcoded "ar"
4. `SourcesCatalogViewModel.kt`: Restore `locale = Locale.getDefault().language.takeIf { it in locales }`

### To allow NSFW content:
1. `AppSettings.kt`: Revert `isNsfwContentDisabled` getter/setter
2. `SourcesCatalogViewModel.kt`: Restore the conditional filtering
3. `WelcomeViewModel.kt`: Remove `.filterNot { it == ContentType.HENTAI }`
4. `SourcesManageFragment.kt`: Restore the NSFW menu item visibility

---

## Version
Modified: March 2026
Base Version: 9.6.6

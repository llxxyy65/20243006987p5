# Chinese Municipalities — Practical 5

Android application for Practical 5: **Resources, Intents, and Dynamic UI in Android**.

| Item | Value |
| --- | --- |
| Student ID | 20243006987 |
| Project name | PracticalFive |
| Language | Java |
| UI | XML layouts |
| Package | `com.example.practicalfive` |
| minSdk / targetSdk / compileSdk | 24 / 34 / 34 |
| AGP / Gradle | 8.5.2 / 8.7 |

## What the app does

1. **Main screen** (`MainActivity`, `activity_main.xml`) shows the title
   *Municipalities in China*, the map image and a single-line `EditText`
   labelled *I want to know about:* with the keyboard Enter key set to **Go**.
2. Pressing Enter validates the input with `TextProcessor`. `beijing`, `BEIJING`
   and `Beijing` all resolve to `Beijing`. Empty input, spaces only and
   unsupported names such as `Guangzhou` show an error and the user stays on the
   main screen.
3. Valid input is passed to the **second screen** (`Municipality`) through an
   explicit `Intent` extra keyed by `MainActivity.EXTRA_MUNICIPALITY`.
4. The second screen re-validates the extra, then sets the name banner
   background, the name text colour, the information text colour, the image and
   the *Visit site* button tint from named resources for that municipality.
5. *Visit site* fires an **implicit** `Intent.ACTION_VIEW` for that
   municipality's official website and handles `ActivityNotFoundException`.

## Requirement mapping

| Hand-out requirement | Where it is implemented |
| --- | --- |
| 4.1 Images | `res/drawable/` — `chinese_municipalities.png`, `beijing.jpg`, `shanghai.jpg`, `tianjin.jpg`, `chongqing.jpg` (all lowercase with underscores) |
| 4.2 Application name | `res/values/strings.xml` → `app_name` = *Chinese Municipalities* |
| 4.3 Launcher icon | `res/mipmap-*/ic_launcher*.png`, `res/mipmap-anydpi-v26/ic_launcher*.xml` (name kept as `ic_launcher`, logo from `app_logo.png`, solid black background) |
| 4.4 Colour resources | `res/values/colors.xml` — `municipality_beijing`, `municipality_shanghai`, `municipality_tianjin`, `municipality_chongqing`, `color_text_light`, `color_text_dark`, `color_default_background` |
| 4.5 Municipality array | `res/values/arrays.xml` → `municipalities_array`, four items, 99–101 words each |
| 5 Main screen | `activity_main.xml`, `MainActivity.java` |
| 6 TextProcessor | `TextProcessor.java` (plain class, no Activity, no layout) |
| 7 Second Activity | `Municipality.java`, `activity_municipality.xml`, registered in `AndroidManifest.xml` |
| 8 Explicit Intent | `MainActivity.openMunicipalityScreen()` |
| 9 Dynamic UI | `Municipality.onCreate()` — `setText`, `setBackgroundColor`, `setTextColor`, `setImageResource`, `setBackgroundTintList` |
| 10 Implicit Intent | `Municipality.openWebsite()` |
| 11 Testing | `app/src/test/java/com/example/practicalfive/TextProcessorTest.java` plus the manual table below |

### Array order

`municipalities_array` follows the order required by the hand-out:

```
0 = Beijing      1 = Shanghai      2 = Chongqing      3 = Tianjin
```

`Municipality.java` defines `INDEX_BEIJING`, `INDEX_SHANGHAI`, `INDEX_CHONGQING`
and `INDEX_TIANJIN` so the correct summary is always paired with the correct
municipality. Do not reorder the array without updating those constants.

### Colour choices

| Resource | Value | Note |
| --- | --- | --- |
| `municipality_beijing` | `#C62828` | red |
| `municipality_shanghai` | `#C2185B` | magenta |
| `municipality_tianjin` | `#37474F` | charcoal |
| `municipality_chongqing` | `#E65100` | bright orange |

All four are deep enough for white text to stay readable on the name banner and
for the information text to stay readable on the light background
(`color_default_background` = `#FBF5F9`). No colour hexadecimal value is repeated
in Java code; Java only references the named resources.

### Note on `chinese_municipalities.png`

The hand-out lists `chinese_municipalities.jpg`, but the supplied file is a PNG
with transparency, so it is kept as `.png` to preserve the transparent map
background. Android resource names are unaffected: the layout uses
`@drawable/chinese_municipalities`.

## Official websites used by *Visit site*

| Municipality | URL |
| --- | --- |
| Beijing | https://english.beijing.gov.cn/ |
| Shanghai | https://english.shanghai.gov.cn/ |
| Chongqing | https://english.cq.gov.cn/ |
| Tianjin | https://en.tj.gov.cn/ |

## Opening the project

1. Open Android Studio → **Open** → select this folder
   (`20243006987-Practical5`, the folder that contains `settings.gradle.kts`).
2. If Android Studio asks about the Gradle wrapper, accept the prompt that uses
   `gradle/wrapper/gradle-wrapper.properties` (Gradle 8.7) — the IDE downloads
   the wrapper JAR and the distribution on first sync. If the sync complains
   that `gradle-wrapper.jar` is missing, open **File → Project Structure →
   Project** and choose *Gradle* as the wrapper source, or run
   `gradle wrapper --gradle-version 8.7` once from a terminal, then sync again.
3. Use a JDK 17 for the Gradle JDK (AGP 8.5 requires it).
4. Run the `app` configuration on an emulator or device (API 24+).

## Manual test table

| Input | Expected result | Result |
| --- | --- | --- |
| `Beijing` | Beijing screen, red banner, red info text, red button, `beijing.jpg` | pass |
| `beijing` | same as above | pass |
| `BEIJING` | same as above | pass |
| `Shanghai` | Shanghai screen, magenta styling, `shanghai.jpg` | pass |
| `Tianjin` | Tianjin screen, charcoal styling, `tianjin.jpg` | pass |
| `Chongqing` | Chongqing screen, orange styling, `chongqing.jpg` | pass |
| `Guangzhou` | error on the input field, stays on the main screen | pass |
| empty | input validation error | pass |
| spaces only | input validation error | pass |
| *Visit site* on each of the four screens | opens the matching official site in a browser | pass |

Run the JVM unit tests with:

```
./gradlew test
```

(or right-click `TextProcessorTest` → Run in Android Studio).

## Submitting

The repository must be public, named `20243006987-Practical5`, and contain at
least 8 meaningful commits. This working copy already has the commits; only the
remote has to be created:

```bash
cd 20243006987-Practical5
git remote add origin https://github.com/<your-username>/20243006987-Practical5.git
git branch -M main
git push -u origin main
```

Create the empty repository on GitHub **without** README, .gitignore or licence,
set visibility to **Public**, then paste the repository HTTPS URL into the
Practical 5 submission link on Canvas.

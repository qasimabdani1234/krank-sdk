# Krank Form SDK — Maven Repository

Public Maven repository for the Krank Form SDK, served via GitHub Pages.
No authentication or token required.

## Installation

**1.** Add the repository in your project's `settings.gradle.kts`:

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven { url = uri("https://qasimabdani1234.github.io/krank-sdk") }
    }
}
```

**2.** Add the dependency in your app module's `build.gradle.kts`:

```kotlin
dependencies {
    implementation("com.krank.sdk:core:1.0.0")
}
```

That's it — all transitive dependencies (Compose, Ktor, …) are resolved
automatically through the included POM. No manifest entries, no permissions,
no Compose/Ktor setup needed in your app.

## Requirements

- `minSdk` 26 or higher
- `compileSdk` 36

## Usage

```kotlin
import com.krank.sdk.FormSdk
import com.krank.sdk.FormSdkContract
import com.krank.sdk.SdkModel
import com.krank.sdk.model.FormResult

class MainActivity : ComponentActivity() {

    private val form = registerForActivityResult(FormSdkContract()) { result ->
        when (result) {
            is FormResult.Submitted -> { /* result.values, result.response */ }
            FormResult.Cancelled    -> { /* user backed out */ }
            is FormResult.Failed    -> { /* result.message */ }
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        FormSdk.init(
            integrationKey = "<your-integration-key>",
            model = SdkModel.CLAUDE,   // or OPEN_AI / GEMINI
        )

        // form.launch(Unit) — whenever you want to show the form
    }
}
```

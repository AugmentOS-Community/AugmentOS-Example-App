# Example AugmentOS App

### Update 2025 - this repo is outdated. To build an [AugmentOS](https://augmentos.org/) app for smart glasses such as *Even Realities G1* and *Vuzix Z100*, check out the new [AugmentOS Example Cloud App Repository](https://github.com/AugmentOS-Community/AugmentOS-Cloud-Example-App).






# Example AugmentOS App (OUTDATED)

This repository provides a **bare-bones example** of how to build a Third-Party App (TPA) for **[AugmentOS](https://www.augmentos.org/)**, the operating system for smart glasses. If you want to get started with building apps for AugmentOS, start here.

---

# How to make an AugmentOS app

## **Super Easy Mode: Clone This Repo**

If you just want to dive in:
1. Clone this repo.
2. [Install AugmentOSLib](#augmentoslib-installation)
3. Build and deploy the app to your AugmentOS Puck or other device running AugmentOS Core.

You're now running an AugmentOS TPA!

---

## **Easy Mode: Start from Scratch**

If you want to start from scratch:
1. Start a fresh Android Studio Project.
2. [Install AugmentOSLib](#augmentoslib-installation)
3. Add the following to your app's `AndroidManifest.xml` file:

```
<service android:name="com.yourpackage.YourAugmentosService"
    android:exported="true">
    <!-- Intent filter required to communicate with AugmentOS -->
    <intent-filter>
        <action android:name="AUGMENTOS_INTENT" />
    </intent-filter>
</service>
```

4. Create a `tpa_config.json` file in `app/src/main/res/raw/`:

```json
{
  "name": "Example App",
  "description": "Example App Description",
  "version": "1.0.0",
  "settings": [
    {
      "key": "enableLogging",
      "type": "toggle",
      "label": "Enable Logging",
      "defaultValue": true
    },
    {
      "key": "username",
      "type": "text",
      "label": "Username",
      "defaultValue": "JohnDoe"
    },
    {
      "key": "volumeLevel",
      "type": "slider",
      "label": "Volume Level",
      "min": 0,
      "max": 100,
      "defaultValue": 50
    }
  ]
}
```

5. Build and deploy the app to your AugmentOS Puck or other device running AugmentOS Core.

You're now running an AugmentOS TPA!

---

## **AugmentOSLib installation**

1. Clone the [AugmentOS repository](https://github.com/AugmentOS-Community/AugmentOS) next to your app's directory (default setup)

2. If you cloned the [AugmentOS repository](https://github.com/AugmentOS-Community/AugmentOS) elsewhere, update the path to AugmentOSLib in `settings.gradle`:
   ```
   project(':AugmentOSLib').projectDir = new File(rootProject.projectDir, '../AugmentOS/augmentos_android_library/AugmentOSLib')
   ```

---

## **How It Works**

### **Service-Based Architecture**
- The core of this app is a foreground service (`ExampleAugmentosAppService`) that extends `SmartGlassesAndroidService`.
- This service allows the app to:
  - Communicate with the AugmentOS Core.
  - Subscribe to data streams like transcription and phone notifications
  - Display content directly on the smart glasses.

## **AugmentOSLib**

The library interacts with AugmentOS in a few ways:
- Data Subscriptions
- Display Requests
- Access & Modify AugmentOS Settings

---

### **Data Subscriptions**

The app can subscribe to a variety of data streams and handle the incoming events:

```java
// Request English transcriptions 
augmentOSLib.requestTranscription("English");
    
// Get them like this
@Subscribe
public void onSpeechTranscriptionTranscript(SpeechRecOutputEvent event) {}
```

```java
// Request translated English transcriptions from Spanish
augmentOSLib.requestTranslation("Spanish", "English");

// Get them like this
@Subscribe
public void onSpeechTranslationTranscript(TranslateOutputEvent event) {}
```

```java
// Request phone notifications
augmentOSLib.requestNotifications();

// Get them like this
@Subscribe
public void onNotificationEvent(NotificationEvent event) {}
```

(COMING SOON)
```java
// Request glasses button taps
augmentOSLib.requestGlassesSideTaps();

// Get them like this
@Subscribe
public void onGlassesSideEvent(GlassesSideTapEvent event) {}
```

(COMING SOON)
```java
// Request smart ring button taps
augmentOSLib.requestSmartRingButtonTaps();

// Get them like this
@Subscribe
public void onSmartRingButtonTapEvent(SmartRingButtonTapEvent event) {}
```
--- 

### Displaying content on smart glasses
      
**Reference cards**:
```java
augmentOSLib.sendReferenceCard("Title", "Body text.");
```
**Bullet point lists**:
  ```java
  augmentOSLib.sendBulletPointList("Title", new String[] {"Point 1", "Point 2"});
  ```
**Centered text**:
  ```java
  augmentOSLib.sendCenteredText("Centered Text Example");
  ```
**Text wall**:
  ```java
  augmentOSLib.sendTextWall("This is a block of text that fills the screen.");
  ```
**Double text wall**
```java
augmentOSLib.sendDoubleTextWall("Top Text", "Bottom Text");
```
**Row cards**:
```java
augmentOSLib.sendRowsCard(new String[] {"Row 1", "Row 2", "Row 3"});
```
**(COMING SOON) Bitmap images**:
```java
augmentOSLib.sendBitmap(myBitmap);
```
**(COMING SOON) Custom layouts via JSON**:
```java
augmentOSLib.sendCustomContent("{ \"custom\": \"layout\" }");
```

---

### Access & Modify AugmentOS Settings

**To start, specify a settings object in your `tpa_config.json`**

```
{
  "name": "Example App",
  "description": "An example app for AugmentOS",
  "version": "1.0.0",
  "settings": [
    {
      "key": "exampleToggleSetting",
      "type": "toggle",
      "label": "This is a toggle",
      "defaultValue": true
    },
    {
      "key": "exampleTextSetting",
      "type": "text",
      "label": "This is a text box",
      "defaultValue": "Some good default here"
    },
    {
      "key": "exampleSliderSetting",
      "type": "slider",
      "label": "This is a slider",
      "min": 0,
      "max": 100,
      "defaultValue": 50
    },
    {
      "key": "selectInfoText",
      "type": "titleValue",
      "label": "This is a select dropdown",
      "value": "Use this to have users select one of a few options"
    },
    {
      "key": "exampleSelectSetting",
      "type": "select",
      "label": "Color Scheme",
      "options": [
        { "label": "Light",  "value": "light" },
        { "label": "Dark",   "value": "dark" },
        { "label": "System", "value": "system" }
      ],
      "defaultValue": "system"
    },
    {
      "key": "selectInfoText",
      "type": "titleValue",
      "label": "This is a multiselect",
      "value": "Use this to have users select one or more of a few options"
    },
    {
      "key": "exampleMultiselectSetting",
      "type": "multiselect",
      "label": "Favorite Colors",
      "options": [
        { "label": "Red",    "value": "red" },
        { "label": "Green",  "value": "green" },
        { "label": "Blue",   "value": "blue" },
        { "label": "Yellow", "value": "yellow" }
      ],
      "defaultValue": ["red", "blue"]
    }
  ]
}
```

**Access settings within your app**
```java
AugmentOSSettingsManager.getBooleanSetting(this, "exampleToggleSetting");

AugmentOSSettingsManager.getStringSetting(this, "exampleTextSetting");

AugmentOSSettingsManager.getSliderSetting(this, "exampleSliderSetting");

AugmentOSSettingsManager.getSelectSetting(this, "exampleSelectSetting");

AugmentOSSettingsManager.getMultiSelectSetting(this, "exampleMultiselectSetting");
```

**Modify settings within your app**
```java
AugmentOSSettingsManager.setBooleanSetting(this, "exampleToggleSetting", true);

AugmentOSSettingsManager.setStringSetting(this, "exampleTextSetting", "New value!");

AugmentOSSettingsManager.setSliderSetting(this, "exampleSliderSetting", 42);

AugmentOSSettingsManager.setSelectSetting(this, "exampleSelectSetting", "Dark");

AugmentOSSettingsManager.setMultiSelectSetting(this, "test", Arrays.asList("Blue", "Red"));
```
---

## **License**

This project is licensed under the MIT License. See the `LICENSE` file for more details.

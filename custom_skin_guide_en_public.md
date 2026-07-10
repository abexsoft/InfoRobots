# Custom Skin Mod Creation & Installation Guide

This guide explains the specifications and steps for creating a custom skin as an independent Satisfactory plugin (Mod) and loading it as an InfoRobot skin in-game using the automatic detection system. [Here](https://github.com/abexsoft/QuinnSkin) is the sample custom skin.

---

## **Overview (Key Concept)**
Due to Satisfactory (UE5) security restrictions (mandatory IoStore encryption signatures), custom skin assets must be packaged as a plugin (Mod) using **`Alpakit`**, the official SML (Satisfactory Mod Loader) build tool, to be loaded into the game.

*   **Skin Creators (Modders)**: Create the skin in the SML development environment, and build the package (signed `.pak`, `.utoc`, `.ucas`) using `Alpakit` for distribution.
*   **General Players (Users)**: Can install the skin simply by placing the distributed skin Mod folder in the `Mods/` directory. And enable 'Scan External Skins' in the game's global settings.

---

## **1. Asset Creation Rules (For Skin Creators)**

Create a new Content Plugin (e.g., `MySkin`) in your SML development project, and design/place assets according to the following rules:

### **Directory Structure**
1.  Under the plugin's `Content` directory, create a folder named exactly **`InfoRobotsSkins`**.
    *   Path structure: `/PluginName/InfoRobotsSkins/`
2.  Inside it, create a folder named with any alphanumeric characters (e.g., **`MySkin`**), which will serve as the unique Skin ID.
    *   Path structure: `/PluginName/InfoRobotsSkins/MySkin/`
    *   ※ The InfoRobot Mod automatically scans for this specific folder structure (`InfoRobotsSkins`), so the folder name must match exactly.

### **Required Assets**
Place the following three assets inside the `/PluginName/InfoRobotsSkins/[SkinID]/` directory:

1.  **SkeletalMesh** (The 3D model itself)
    *   The 3D mesh data of the robot.
2.  **AnimBP** (Animation Blueprint)
    *   Controls the motion of the robot during movement and data collection.
    *   **【C++ Auto-Synchronized Variables】**: The InfoRobot Mod automatically updates the following variables in the AnimBP every frame. You must declare these variables inside the AnimBP Event Graph, **matching the case exactly**:
        *   **`mSpeed` (Float)**: The current movement speed of the robot (in cm/s). Used for controlling locomotion blend spaces.
        *   **`mIsCollecting` (Boolean)**: Data collection status. Automatically set to `true` when the robot arrives at a node or station and plays the data-gathering animation. Used to drive state machine transitions.
3.  **SkinDescriptor** (Configuration Asset)
    *   **Base Class**: **`Object`** or **`PrimaryDataAsset`** (Select this as the parent class when creating a new Blueprint class.).
    *   Asset Name: **`DA_[SkinID]`** (e.g., `DA_MySkin`)
    *   **Configuration Variables to Add**:
        > [!IMPORTANT]
        > After creating variables, make sure to compile. Otherwise, modifying default values is not possible.

        You can safely override the robot's appearance and behavior by adding the following variable names and types:
        *   `mSkinName` (Text): The display name of the skin in the UI (e.g., `My Skin`. Supports spaces and localization).
        *   `mSkinDescription` (Text): The description of the skin displayed in the UI.
        *   `mSkeletalMesh` (SkeletalMesh Object Reference): Assign your custom SkeletalMesh asset.
        *   `mAnimClass` (AnimInstance Class Reference/Class): Assign your custom AnimBP asset.
        *   **`mMaxWalkSpeed` (Float, Optional)**: The maximum walk speed of the robot when this skin is applied. If omitted or set to `0`, it defaults to the system default (`300.0`).
        *   **`mCameraHeightFromGround` (Float, Optional)**: The height of the first-person camera (eye level) from the ground (in cm) when hijacked by the player. If omitted or set to `0`, it defaults to the system default (`200.0`). (Automatically converted to relative Z offset in C++ based on the capsule center).
        *   **`mCameraForwardOffset` (Float, Optional)**: Forward offset of the first-person camera (in cm. Default: `30.0`). Used to fine-tune the camera position to prevent the body or head mesh from clipping into the view.

---

## **2. Packaging and Distribution (For Skin Creators)**

By default, Unreal Engine excludes "unreferenced assets" from cooking. **You must configure the following project setting first.** Otherwise, your skin assets will be left out of the packaged `.pak` file.

### **Required Cooking Setting (In Unreal Editor)**
1.  Open **"Edit" ➔ "Project Settings"**.
2.  Navigate to **"Project" ➔ "Packaging"**.
3.  Expand the Advanced settings (click the arrow) and locate **"Additional Asset Directories to Cook"**.
4.  Add a new element to the array and specify your skin folder path.
    *   Example: `Plugins/MySkin/Content/InfoRobotsSkins`

### **Building and Packaging**
1.  From the **"Alpakit"** menu (or command) in the Unreal Editor, select your plugin and build it.
2.  This will automatically build the signed IoStore container (`.pak`, `.utoc`, `.ucas`).
3.  Compress the generated plugin folder (e.g., `<Satisfactory>/FactoryGame/Mods/MySkin/ or /Mods/GameFeatures/MySkin/`) into a zip archive and distribute it to users.

---

## **3. Installation Steps for Users (Players)**

Players only need to place the distributed skin Mod folder into the game's `Mods` directory.

### **Target Directory Structure**
```text
<Satisfactory_Installation_Folder>/FactoryGame/Mods/ (or +GameFeatures/)
  └── [Skin_Plugin_Name]/   (e.g., MySkin folder)
        ├── [Plugin_Name].uplugin
        └── Content/
              └── Paks/
                    └── Windows/
                          ├── [Plugin_Name]FactoryGame-Windows.pak
                          ├── [Plugin_Name]FactoryGame-Windows.utoc
                          └── [Plugin_Name]FactoryGame-Windows.ucas
```

---

## **4. Auto-Detection and Loading Mechanism**

Upon game startup and skin application, the C++ backend of the InfoRobots Mod automatically executes the following logic:

1.  **Check the 'Scan External Skins' option**:
    InfoRobot Mod scans only when 'Scan External Skins' is enabled in the game's global settings.
2.  **Plugin-Specific Directory Scanning**:
    Scans all loaded plugins (e.g., `MySkin`) and auto-detects virtual paths under **`/[PluginName]/InfoRobotsSkins`** in the asset registry. It ignores irrelevant directories, minimizing game startup overhead.

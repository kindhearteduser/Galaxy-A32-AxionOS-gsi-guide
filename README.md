# Samsung Galaxy A32 4G (MTK) - AxionOS 2.9 GSI Installation Guide - [Support Me](https://github.com/kindhearteduser/Galaxy-A32-AxionOS-gsi-guide#-support-the-project)
---
I am not responsible for bricked devices, dead SD cards, thermonuclear war, or you getting fired because the alarm app failed. Please do some research if you have any concerns about features included in before flashing it! YOU are choosing to make these modifications, and if you point the finger at me for messing up your device, I will laugh at you.
---
---

This repository provides a foolproof, step-by-step guide to installing **AxionOS 2.9 GSI** on the Samsung Galaxy A32 4G. 

By following this exact layout, you will bypass Samsung's strict dynamic partition limits, disable `vbmeta` verification, and boot a highly customized open-source ROM with **fully working internal storage encryption** and absolute zero lag.

---

## ⚠️ CRITICAL NOTICE FOR SMS USERS (PLEASE READ BEFORE FLASHING)
* **ROOT IS MANDATORY FOR SMS:** If you want to send or receive SMS on this GSI, **YOU MUST ROOT YOUR DEVICE.** Without root and the specific Magisk module mentioned below, SMS will NOT work at all.

---

## ⚡ Performance & Review Note
* **UI Experience:** Extremely fluid with zero lag.
* **Pro Tip:** Disable "System Blurs" / "Window Blurs" in settings. Keeping blur enabled might cause tiny GPU drops on the Mali-G52.

---

## 🟢 What's Working:
* everything except those that do not work

## 🚫 What's NOT Working (Known Issues):
* **Fingerprint Sensor:** Broken. The GSI source literally thinks the device has a physical rear fingerprint sensor (lmao), so the under-display optical sensor is completely ignored. Just use a PIN, Pattern, or Face Unlock.
* **VoLTE** It may vary from one operator to another, but it most likely will not work. It is recommended that you disable the VoLTE feature in your operator settings.
**Always On Display** For some reason, there's no such option on the phone.


---

## 🛠️ The Crucial SMS Fix
Like most GSIs, internal network configurations break texting. To completely fix the SMS not sending/receiving issue:
1. Ensure you follow **Step 5** in the installation guide to root your device via Magisk.
2. Once booted into the system, download and flash the **[https://xdaforums.com/t/module-a325f-m-fix-sending-sms-in-u-custom-rom.4718379/]** via the root app.
3. Reboot your phone, and your SMS functions will work flawlessly!

---

## 🚀 Step-by-Step Installation

### 📋 Prerequisites & Downloads
* Ensure your phone is on the **latest Stock ROM firmware and Binary version**.
* [Download **Odin**](https://samfw.com/Odin/Samfw.com_Odin3_v3.14.4.zip)
* [Download the vbmeta_disabled_r.tar](https://xdaforums.com/attachments/vbmeta_disabled_r-tar.5236537/?hash=f7249adaefe16f3aeac3256a63063f0a)
* [Download **TWRP**](https://github.com/LumiROM/a32-useful-things/raw/refs/heads/main/Recoverys/TWRP/TWRP-3.7.0_erofs-a32-20251227.tar) for Galaxy A32 4G.
* [Download **Magisk**](https://github.com/topjohnwu/Magisk/releases)
* [Download **Platform-Tools**](https://developer.android.com/tools/releases/platform-tools?hl=tr#downloads)
* [Download the **AxionOS 2.9 GSI Image**](https://github.com/Doze-off/axion_aosp/releases/tag/2026-10-17) (Choose your variant: GApps, microG, or Vanilla). Extract the `.img` file from the archive.

---

### Step 1: Unlock Bootloader & Enable Options
1. Go to **Settings -> About Phone -> Software Information**.
2. Tap **Build Number** 7 times until "Developer options have been enabled" appears. Enter your password if prompted.
3. Go back to the main Settings menu, open **Developer Options**, and toggle on **OEM Unlocking** and **USB Debugging**.
4. Connect your phone to the PC and restart it. 
5. As soon as the screen goes black, press and hold **Volume Up + Volume Down** buttons simultaneously until the Download Mode screen appears.
6. Long-press the **Volume Up** button to enter the bootloader unlock screen. 
7. Release the button, then press **Volume Up** once to confirm: `Unlock Bootloader? -> Yes`.
8. The device will wipe and reboot. Immediately press and hold **Volume Up + Volume Down** again to boot back into Download Mode. Press **Volume Up** once to continue.

---

### Step 2: Flash VBMeta & TWRP via Odin
1. Open **Odin** on your PC.
2. Click the **AP** slot, select your patched `vbmeta_disabled_R.tar` file, and flash it.
3. As soon as the screen goes black after flashing, immediately press and hold **Volume Up + Volume Down** to enter Download Mode once again.
4. Now, click the **AP** slot in Odin again, select the `TWRP-3.7.0_erofs-a32-20251227.tar` file, and flash it.
5. When the screen goes black, immediately press and hold **Volume Up + Power** buttons until the bootloader warning screen appears. Release them and tap the **Power** button to continue booting into TWRP.

---

### Step 3: FastbootD & Partition Modification
1. Once inside TWRP, wait a few seconds, go to **Wipe -> Format Data**, type `yes`, and confirm.
2. Go back to the TWRP main menu, select **Reboot**, and tap **Fastboot** (This drops you into Fastboot mode).
3. Tap the physical **Power** button to pass the bootloader warning if the screen pauses.
4. Extract the **Platform-Tools** zip on your PC. Inside the folder where `adb.exe` and `fastboot.exe` are located, click the file explorer address bar, type `cmd`, and press Enter.
5. In the CMD window, execute the following commands one by one to free up logical space and wipe user data:
   ```bash
   fastboot delete-logical-partition product
   fastboot erase userdata
   fastboot erase system
   ```

---

### Step 4: Flashing AxionAOSP GSI
1. Copy the full path of your extracted AxionOS `.img` file (e.g., `"C:\Users\user\Desktop\Axion-2.9-GSI_treble_arm64-ab-GAPPS.img"`).
2. In the CMD window, type the following command and hit Enter:
   ```
   fastboot flash system "paste_your_copied_image_path_here"
   ```
3. Once the flashing process completes successfully, type this command to return to recovery:
   ```bash
   fastboot reboot recovery
   ```
4. Bypass the bootloader warning screen using the **Power** button again.

---

### Step 5: Rooting (MANDATORY FOR SMS FIX) & Final Boot
1. When TWRP boots up, go to **Advanced -> ADB Sideload** and swipe to start it.
2. On your PC CMD window, execute the following command to root the device (so you can install the SMS fix later):
   ```
   adb sideload "path_to_magisk.zip"
   ```
3. Once the sideload finishes, return to the TWRP main screen.
4. Go to **Wipe -> Format Data**, type `yes`, and confirm. *This final format ensures encryption initializes properly.*
5. Tap **Reboot System**.

**Congratulations! You have successfully installed AxionAOSP on your Galaxy A32 4G! Now open the Magisk app and flash your SMS module.** 🚀

---
## ☕ Support the Project

If this project helped you save time or unbrick your device, feel free to support my work! Since I don't use traditional banking, I accept donations exclusively through crypto.

### 🌐 EVM Networks (Preferred & Cheapest)
*Supports: Ethereum, BNB Chain, Polygon, Arbitrum, Optimism, Base, Linea, Monad*
```text
0xe26b507090707acffe16c34a8c6cfca1ee3d6ff1
```

---


# Assignment 1

## Task 1: Review feature description and requirements:
> **Features:**
> 1. Switching between: Noise Cancellation and Transparency Mode
> 2. Saving the user's last selected mode
> 3. Automatically restoring the last selected mode when the device reconnects
> 4. Showing battery level for: left earbud, right earbud, charging case
> 5. Showing connection status in the app
> 6. Allowing listening mode change only when the product is connected
> 7. Preventing listening mode switching when the battery is critically low
> 8. Supporting both iOS and Android
>
> **Expected behaviour:**
> - The user can change listening mode only when the device is connected
> - The selected listening mode should be applied to the device within 2 seconds
> - The selected listening mode shown in the app must match the actual mode active on the device
> - The last selected mode should persist after app restart
> - The last selected mode should be restored after reconnect
> - Battery levels shown in the app should reflect the device state correctly
> - If the battery is below 5%, mode switching is blocked, and the app should display a clear message
> - The feature should behave consistently on both iOS and Android
>
> **Real-world context:**
> - Some users update the app but not the device firmware
> - Bluetooth connection quality may vary in real environments
> - Users may switch between phone call audio and music playback
> - Users expect a quick, reliable response because this is a premium product

The feature description was reviewed and the following notes were made:
- The transition behaviour between modes, apart from the response time, is not specified. Given that this is a premium product, the user expects a smooth transition between modes, which can be implemented e.g., with a crossfade. Furthermore, a response time of 2 seconds might seem a bit sluggish. Reducing it to 1 second might improve the perceived quality of the feature.
- After the listening mode is changed on the app, how long does it take to update the listening mode displayed on the app? I would argue that 1 second is a maximum.
- Given the insight that some users might update the app but not the device firmware, there should be a fallback in place that bypasses the Adaptive Listening Modes feature in the app for older device firmware versions that do not support it. Adding a notification in the app that informs the user of this new feature and includes a button to update the firmware is recommended.
- It might be a good idea to allow the user to choose what mode they prefer during phone calls. Noise cancellation may alter the way a user perceives their own voice, which can affect their speech production and create a negative user experience. Hence, some users may prefer automatically switching to Transparency Mode during phone calls.
- Can the earbuds be connected to multiple devices simultaneously or only one device at a time?
- Is there a Desktop (Windows/macOS) companion app or can the modes only be controlled from a mobile device?
-  What are the latency constraints for the Transparency mode? Since the processed sound will be mixed with the direct sound, we need to be aware of possible echo and comb-filtering, which could degrade the listening experience.
- (*) We are missing audio quality requirements, such as "there should be no audible artifacts (clicks, drop-outs, pops) associated with switching between modes".
- (*) Can the listening modes be switched from the earbuds themselves (e.g., with a button) or only on the app?
- (*) "If the battery is below 5 \%, mode switching is blocked, and the app should display a clear message". Which battery level is being referred to here, right, left earbud or charging case? Does this apply when only one of them is below 5 \% or all of them?
- (*) Is the last used mode stored for each source device separately?
- (*) Is mode switching allowed when wearing only one of the earbuds?

## Task 2: Test approach

> Describe your test approach for this feature:
> - what you would test first and why
> - what areas would you prioritize most
> - what you would focus on in exploratory testing
> - what regression areas would you consider important
> - how you would think about coverage across iOS and Android

I would prioritize the updating process, the sound quality and mode switching. I would follow a risk-based prioritisation strategy. starting with testing that updating the firmware and the app works as intended. This includes testing that the product works with the latest firmware installed, but not the latest app version and vice versa. The last thing we want is for the update to break the product. Then, I would test each mode for sound quality while listening to music and talking on the phone. If the sound quality is not up to the product's standard, then that should be fixed first as it is likely the first thing the user will notice after updating. When the sound quality of each mode has been verified, I would test the switching between modes, both during audio streaming and phone calls to make sure the switching works in both states (A2DP and HFP), and that it is smooth with no audible artifacts during the switch or after. 

In exploratory testing, I would test edge cases and see if I can cause the feature to break. I would test what happens when the bluetooth connection is lost, and observe whether the last saved mode is loaded when the device reconnects. I would restart the app and test whether the correct mode is loaded. I would test what happens when I rapidly switch between modes. I would test what happens when the earbuds' battery is low (< 5 pct). I would stress test the feature in a real-world environment with a lot of competing devices (e.g., café or train station) and make sure it works as intended.

In terms of regression testing, I would make sure all of the already implemented features (buttons, controls, connectivity, etc.) work in each of the listening modes and that they do not cause audible artifacts (e.g., when skipping songs, pressing play or pause).

I would spend equal time testing iOS and Android. Both need to be tested since the behaviour on the two platforms can vary. (*) Special attention should be paid to platform-specific risk areas.

## Task 3: Test scenarios
> Write between 5–10 high-value test scenarios for this feature.

1. Update earbud firmware and app version.
2. Update earbud firmware and use with old app version.
3. Update app and use with old earbud firmware.
4. Switch between listening modes while listening to music.
5. Switch between listening modes during phone call.
6. Disconnect and reconnect earbuds and check listening mode.
7. Restart app and check listening mode.
8. Attempt to switch listening mode when earbud battery is below 5 \%.
9. Attempt to switch listening mode when earbuds are disconnected.
10. (*) Mode switching while wearing only one earbud and then inserting the other.

Each of these should be performed for both iOS and Android.

## Task 4: Test cases
> Write at least 2 detailed test cases in a standard format.

**Test 1**

Title: Switch between listening modes while listening to audio on iOS device.

Preconditions:
- Earbuds (HW version: X, FW version: Y) are connected to iOS device (device model: A, iOS version: B).
- Earbuds battery is above 5 \%.
- Earbuds are in Transparency Mode (which is correctly displayed on the iOS app).
- Audio is playing in earbuds from iOS device (earbuds are in A2DP state).

Steps:
1. Open the iOS app and press the Noise Cancellation Mode button.
2. Wait 5 seconds.
3. On the iOS app, press the Transparency Mode button.
4. Wait 5 seconds.

Expected result:
The earbuds switch modes within 2 seconds of pressing the button on the app. The transitions are smooth (crossfade) and there are no audible artifacts during or after the switches. The sound quality of the audio is not affected by the transitions and there are no dropouts or pauses in the audio. The active listening mode is updated on the app (display) within 1 second of pressing the button.

**Test 2**

Title: Disconnect and reconnect earbuds to Android device and check listening mode.

Preconditions:
- Earbuds (HW version: X, FW version: Y) are connected to Android device (device model: A, Android version: B).
- Earbuds battery is above 5 \%.
- Earbuds are in Transparency Mode (which is correctly displayed on the Android app).

Steps:
1. Open the Bluetooth settings on the Android device.
2. Click on the name of the connected earbuds and press the "Disconnect" button.
3. Wait 5 seconds.
4. Click on the name of the earbuds and press the "Connect" button.
5. Wait 5 seconds.
6. Open the Android app and observe the listening mode.
7. On the Android app, press the Noise Cancellation Mode button.
8. Repeat steps 1-6.
   
Expected result:
The listening mode is maintained after disconnecting and reconnecting the earbuds, both on the earbuds themselves and on the app display.

## Task 5: Bug report based on observed issue
> During testing, you observe the following issue:
> The user selects Transparency Mode in the app while the earbuds are connected. The app immediately updates and shows Transparency Mode as active. However, the earbuds remain in Noise Cancellation for around 8–12 seconds. In some cases, after placing the earbuds back in the case and reconnecting, the app still shows Transparency Mode, but the earbuds are actually in Noise Cancellation Mode.

**Bug report**

Title: Updating listening mode on earbuds is late / unsuccessful

(*) Priority: High

Environment:
Earbuds (HW version: X, FW version: Y), iOS/Android mobile device (device model: A, iOS/Android version: B)

Preconditions:
- Earbuds are connected to the mobile device and inserted in tester's ears.
- Earbuds battery is above 5 \% (assumed).
- Earbuds are in Noise Cancellation Mode (which is correctly displayed on the mobile app).
  
Steps:
1. Open the mobile app and press the Transparency Mode button.
2. Wait for 15 seconds.
3. Put the earbuds in the case.
4. Wait 5 seconds.
5. Remove the earbuds from the case and insert them into your ears.
6. Open the mobile app and check the active mode displayed on the app.
7. Observe the actual mode of the earbuds by listening.
   
Expected result:
The earbuds switch to Transparency Mode within 2 seconds of pressing the Transparency Mode button. After the earbuds are disconnected and reconnected, they are still in Transparency Mode. The app shows Transparency Mode after the button has been pressed.

Actual result:
The earbuds switch to Transparency Mode around 8-12 seconds after pressing the Transparency Mode button. In some cases, after the earbuds are disconnected and reconnected, they are back in Noise Cancellation Mode. The app shows Transparency Mode after the button has been pressed.

Reproducibility: Intermittent - observed in X of Y attempts; needs further investigation (placeholder, need more info for this one)

(*) Logs/attachments: Snoop logs, video

## Task 6: Feature exploration
> Describe how you would explore this feature beyond scripted testing. Please include the kinds of real-world situations you would investigate for headphones/earbuds.

For the Noise Cancellation Mode, I would test whether they cancel out noise across the whole audible frequency range, or whether they are more vulnerable to some frequencies than others. I would also test how well they cancel out sound when moving around, since most noise cancellation algorithms use some sort of adaptive filter that may be sensitive to rapidly changing environments.

For the Transparency Mode, I would test how "transparent" they sound, i.e. how well they recreate the sensation of not wearing earbuds, since that is the goal of this mode. I would have conversations and listen to music with the earbuds in Transparency Mode and observe how natural the sound is.

Furthermore, I would test both listening modes for how they handle problematic sounds such as wind noise and feedback.

(*) I would also stress-test the feature in environments with high RF interference (e.g., crowded 2.4 GHz areas) and in moving vehicles (e.g., a train) to test connectivity robustness.

## Task 7
> Assume these issues were found near release:
> - On Android, listening mode changes sometimes take 4 seconds instead of 2 seconds.
> - On iOS, the battery-low warning is present, but the wording is confusing.
> - In 1 out of 25 reconnect attempts, the app restores the previous mode visually, but the earbuds actually reconnect in a different mode.
>
> Would you recommend: release, conditional release, or no release? Explain your reasoning.

I would categorize the first and last issues as blockers, since they sound rather common ('sometimes' and 1 out of 25 = 4 \%) and are likely to diminish the perceived quality of the product. A response time of 4 seconds is sluggish for premium earbuds and the user is likely to lose trust in the product if there is a discrepancy between what they hear in their earbuds and what is shown on the app. The second issue sounds like a quick fix.

If we still have some time until the release day and the development team can fix these issues while giving the QA time to re-test, then I would recommend a conditional release. If we are very close to the intended release date I would recommend postponing the release until we have fixed the blockers and re-tested the whole system.

# Assignment 2

> In the provided Wireshark trace of a Bluetooth audio streaming session between a headset and a smartphone, the user reports audio cuts after a few seconds. Provide a step-by-step approach on how you would analyze the issue. Please refer to the contents of the file: `A2DP_audio_drops.7z`.

I would start by making a timeline of the most important events captured by the log. Most of the entries in the log are probably audio packets (marked HCI_H4), so we can filter them out. The user reports audio cuts after "a few seconds" so we can focus on the first 10 seconds to begin with.

### Findings by protocol

HCI_CMD:
"Sent Vendor Command..." sent 4 times between 6.435222 s and 6.699312 s.

HCI_EVT:
"Rcvd Mode Change" at 6.435127 s.
"Rcvd Command Complete (Vendor Command ...))" 4 times from 6.435346 s to 6.575702 s.
"Rcvd Unknown 0xeb" 4 times from 6.622948 s to 6.735369 s.

ATT:
No ATT messages.

L2CAP:
"Rcvd/Sent Connection oriented channel" sent 7 times total between 6.435196 s and 6.622860 s.

AVDTP:
No AVDTP messages.

### Timeline

Timeline (between 6.435222 s and 6.735369 s):
1. Rcvd Mode Change
2. Rcvd Connection oriented channel
3. Sent Vendor Command 0x01C1
4. Sent Connection oriented channel
5. Rcvd Command Complete (Vendor Command 0x01C1)
6. Sent Vendor Command 0x01C5
7. Sent Vendor Command 0x01C1
8. Sent Vendor Command 0x01C2
9. Sent Connection oriented channel
10. Rcvd Connection oriented channel
11. Rcvd Command Complete (Vendor Command 0x01C5)
12. Rcvd Command Complete (Vendor Command 0x01C1)
13. Rcvd Command Complete (Vendor Command 0x01C2)
14. Rcvd Connection oriented channel
15. Rcvd Connection oriented channel
16. Rcvd Unknown 0xeb (4 times)
17. Sent Vendor Command 0x01C4

In addition, the log contains a bunch of HCI H4 "Sent Unknown HCI packet type 0x41/0x40 in between and the occasional HCI EVT "Rcvd Number of Completed Packets".

### Conclusion

It appears that the reported dropouts may be caused by a Mode Change ca. 6.4 s into the trace, which triggers a series of vendor and connection-related commands. 
I would report this bug, including my analysis notes and timeline in an attachment.

# Assignment 3

> The attached zip file `TWS_User_Scenario_EXT.7z` contains Wireshark logs for a TWS Bluetooth audio system.
> - Analyse the logs and find out the roles of each device.
> - Find out the primary user scenario covered in the logs.
> - Explain the analysis steps.

My strategy is to use Wireshark to filter the logs by different protocol types, see what devices communicate with what other devices using the different protocols, and then interpret the findings.

### Findings by protocol

**HCI_CMD** (host to controller messages):
- Device 3 has a bunch of HCI commands from "host" to "controller".
- Device 2 also has a bunch of HCI commands from "host" to "controller".
- Device 1 also has a bunch of HCI commands from "host" to "controller".

**ATT** (BLE control and small-data exchange):
- Device 3 exchanges ATT messages with iPhone and Amadeus Audio Link (earbud?)
- Device 2 exchanges ATT messages with iPhone and the Beo Grace Case.
- Device 1 exchanges ATT messages with iPhone and the Beo Grace Case.

**L2CAP** (channel-multiplexing layer that AVDTP runs on top of):
- Device 3 has no L2CAP messages
- Device 2 exchanges L2CAP messages with other earbud (sometimes source and destination have same MAC address?)
- Device 1 exchanges L2CAP messages with iPhone and Intel device (laptop?)

**AVDTP** (Classic BT audio streaming and media control):
- Device 3 has no AVDTP messages.
- Device 2 has no AVDTP messages.
- Device 1 exchanges AVDTP messages with Intel device and iPhone. Appears to be a multipoint connection, since timing of messages to each source overlaps.

*Note*: earbuds sometimes called "Amadeus Audio Link" and sometimes "Swiss's Grace".

### Interpretation

This is most likely a setup of a pair of earbuds and a charging case:
- Device 1 is the primary earbud which receives audio streams from an iPhone 17 Pro and an Intel Device (probably a laptop). It exchanges BLE control messages with the charging case and iPhone, but not the Intel device, indicating that the status/configuration of the earbuds can be accessed/managed on the iPhone (possibly with a companion app or integrated iOS controls), but not on the Intel laptop (no companion app?).
- Device 2 is the secondary earbud which receives audio via a sideband from the primary earbud.
- Device 3 is the earbud charging case, which communicates with the phone and earbuds using BLE (no audio passes through the case, only a control coordinator).

### Primary user scenario

The primary scenario is most likely a user listening to audio, sometimes from an iPhone and sometimes from an Intel laptop, on TWS earbuds, with the case providing BLE-based status coordination


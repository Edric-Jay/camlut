Project Prompt: camLUT Pro — Professional Cinematic Camera App
1. Project Overview
Build a production-quality, mobile-first camera application called camLUT Pro, inspired by the minimalist aesthetic of Hayata Sato's camLUT concept at https://avantgr.netlify.app/.
This must be a real camera application, NOT a photo editor.
The main experience should resemble a professional mirrorless camera with cinematic color science, a live viewfinder, manual exposure controls, focus controls, white balance, and real-time LUT previews.
Users must be able to compose a shot, adjust camera settings, preview their selected color grade in real time, and take photographs directly from the camera interface.
The design should combine:
The simplicity of a premium Japanese digital camera.
The cinematic color aesthetic of independent filmmakers.
The tactile controls of a professional mirrorless camera.
A clean, understated interface inspired by analog film photography.
Prioritize actual camera functionality over decorative UI. Do not simulate professional controls that do not affect the camera or image processing.
2. Platform and Technical Architecture
Build a responsive mobile-first application with a clear path to native camera functionality.
Preferred technology stack
React and TypeScript.
Vite for the web development environment.
Tailwind CSS for styling.
WebRTC getUserMedia() for browser camera access.
Canvas or WebGL for real-time color processing where supported.
Web Workers for computationally intensive image processing.
IndexedDB for local storage of settings, presets, and captured images.
A native camera integration for Android and iOS when browser APIs are insufficient.
For a native-capable implementation, evaluate:
Capacitor Camera and native plugins.
Android CameraX with Camera2 interop for manual camera controls.
iOS AVFoundation for exposure, focus, white balance, and supported capture formats.
Choose the architecture based on the required features. A browser-only implementation must not pretend to provide native camera capabilities that the browser does not expose.
Keep the camera engine separate from the interface so the app can use a browser camera backend or a native camera backend.
3. Main Camera Interface
The camera viewfinder must be the primary screen and occupy most of the display.
Top control bar
Include:
camLUT Pro logo.
Camera status indicator.
Flash/torch toggle.
Front/rear camera switch.
Settings menu.
Battery status only when available through the platform.
Remaining storage or capture count when available.
Live viewfinder
Display:
Live camera feed.
Optional rule-of-thirds grid.
Optional golden-ratio grid.
Center crosshair.
Level indicator for horizon alignment.
Focus point indicator.
Exposure and focus status.
Selected LUT preview.
Zoom indicator.
Histogram overlay.
Zebra exposure warnings.
Highlight and shadow clipping indicators.
Optional false-color exposure visualization.
Optional focus peaking where supported by the rendering pipeline.
Provide toggles to hide overlays for a clean composition.
Bottom controls
Include:
Recent capture thumbnail.
Large physical-camera-style shutter button.
Video/photo mode switch.
Front/rear camera switch.
Selected LUT name.
Main exposure summary.
Quick-access manual controls.
The shutter button must take a photo. The thumbnail must open the camera's capture gallery, not an unrelated file-upload workflow.
4. Professional Manual Camera Controls
Create a dedicated Pro Mode panel with clearly labeled controls, live numeric values, and tactile horizontal dials.
A. Shutter speed
Support the following conventional shutter-speed values when the hardware permits:
Auto
1/8000
1/4000
1/2000
1/1000
1/500
1/250
1/125
1/60
1/30
1/15
1/8
1/4
1/2
1 second
2 seconds
4 seconds
8 seconds
Bulb, only where genuinely supported
Requirements:
Apply the selected exposure duration to the actual camera.
Display the current shutter speed prominently.
Update the exposure meter as settings change.
Indicate unavailable shutter speeds.
Respect sensor, hardware, and platform limitations.
Warn about motion blur and camera shake at slower shutter speeds.
For web implementations, do not claim that setting a CSS value or applying a filter changes the camera sensor's shutter speed.
B. ISO
Provide a manual ISO selector with common values:
Auto
ISO 50
ISO 64
ISO 100
ISO 200
ISO 400
ISO 800
ISO 1600
ISO 3200
ISO 6400
ISO 12800
ISO 25600
Use the actual ISO range reported by the device. These are target values, not a guarantee that every camera supports every value.
Requirements:
Change sensor sensitivity through the real camera API.
Display the current ISO.
Show Auto ISO limits and configurable minimum/maximum values when supported.
Update exposure metering.
Provide a warning when high ISO may increase image noise.
Do not confuse digital brightness adjustment with actual sensor ISO.
C. Exposure compensation (EV)
Provide an exposure compensation dial from -3 EV to +3 EV in 1/3 EV increments, with a configurable range when supported.
Display values such as:
-2.0 EV
-1.0 EV
-0.3 EV
0.0 EV
+0.3 EV
+1.0 EV
+2.0 EV
Requirements:
Apply compensation through the camera's exposure control API.
Show a live exposure meter and highlight clipping warnings.
Support resetting to 0 EV.
Distinguish exposure compensation in Auto mode from actual manual exposure settings.
Never imply that EV compensation overrides fully manual shutter speed and ISO unless the camera backend actually supports that behavior.
D. Focus control
Provide:
Continuous autofocus.
Single autofocus.
Manual focus where supported.
Tap-to-focus on the viewfinder.
Focus lock.
Focus distance display where available.
Macro or close-focus mode where supported.
Focus peaking overlay where supported.
Optional focus magnification.
Front/rear camera-specific capability detection.
Manual focus must use the camera's real focus-distance control where available. A software sharpening effect must never be presented as manual focus.
Provide a visual focus indicator with:
White: searching.
Green: focus confirmed.
Amber: focus unavailable or uncertain.
Do not claim that focus has been locked if the device does not expose that capability.
E. White balance
Provide preset white balance modes:
Auto
Daylight
Cloudy
Shade
Tungsten
Fluorescent
Flash
Custom Kelvin
Kelvin range: approximately 2,000 K to 10,000 K, restricted to actual device capabilities when applicable.
Requirements:
Apply real sensor white balance when supported.
Show the selected Kelvin temperature.
Support white balance lock.
Offer tint adjustment only when supported by the capture pipeline.
Clearly distinguish actual camera white balance from a software color-temperature preview.
F. Aperture
Display the actual lens aperture when the hardware exposes it.
Allow manual aperture selection only if the device has an adjustable aperture and the native camera API supports changing it.
Do not create a fake aperture slider on phones with fixed-aperture lenses.
When aperture is fixed, show the actual aperture as read-only information if available.
G. Zoom and lens selection
Support:
0.5× ultrawide where available.
1× main lens.
2×, 3×, or other optical lens options when available.
Continuous zoom within supported ranges.
Smooth zoom control.
Focal-length display when available.
Digital zoom clearly identified as digital.
Prefer switching to a physical camera lens over digital cropping when appropriate.
H. Metering and exposure modes
Include:
Matrix/evaluative metering.
Center-weighted metering.
Spot metering where supported.
Program Auto.
Shutter Priority.
ISO Priority if supported by the implementation.
Manual Exposure.
Clearly show the active exposure mode and disable combinations the hardware cannot support.
5. Cinematic LUT System
Color looks must be previewed in the live camera viewfinder before the user presses the shutter.
Create a horizontal, swipeable film-look carousel with small preview thumbnails.
Include these default looks:
Natural — restrained contrast and realistic colors.
Cinema — warm highlights and controlled shadows.
Golden Teal — warm skin and teal-leaning shadows.
Deep Blue — cool shadows and dense blacks.
Clean Blue — cooler neutrals and clear highlights.
Faded Film — muted colors and lifted blacks.
Kodak-inspired Warm Film — warm highlights and gentle color response.
Fuji-inspired Soft Film — restrained saturation and green-sensitive tonal styling.
Portra-inspired Portrait — soft contrast and natural-looking skin tones.
Eterna-inspired Cinema — subdued saturation and cinematic tonal separation.
Bleach Bypass — high contrast and reduced saturation.
Monochrome — black-and-white rendering with adjustable contrast.
These should be original LUT-inspired looks, not falsely advertised as exact manufacturer color profiles.
Requirements:
Apply the selected look to the live preview.
Save the selected look into the captured image.
Preserve the original sensor capture internally when practical.
Provide a toggle for clean, ungraded preview.
Allow LUT intensity adjustment from 0% to 100%.
Support importing standard .cube LUT files.
Support 1D and 3D LUTs where practical, with documented size and format limits.
Use GPU-based processing when supported.
Keep preview latency low.
Avoid blocking the shutter while processing the preview.
Support custom LUT names, favorites, and recent looks.
The capture pipeline must distinguish between the original image, the camera preview, and the final graded photograph.
Where RAW capture is supported, preserve the original RAW file and generate a separate graded JPEG preview or export.
Do not claim a processed preview is a true RAW file with the grade embedded.
6. Additional Professional Tools
Implement these tools as functional overlays where possible.
Histogram
RGB histogram.
Luminance histogram.
Live updates.
Highlight and shadow clipping indicators.
Zebra stripes
Adjustable highlight threshold.
Optional shadow threshold.
Clear visual indication of clipped or near-clipped areas.
Focus peaking
Configurable sensitivity.
Configurable highlight color.
Use actual image detail analysis to identify in-focus edges.
Make clear when the feature is an image-analysis estimate rather than a sensor focus-confirmation signal.
Composition aids
Rule of thirds.
Golden ratio.
Center crosshair.
Horizon level.
Safe-frame guides.
Timer and burst
2-second timer.
5-second timer.
10-second timer.
Burst capture where supported.
Self-timer and remote shutter
Bluetooth remote support if available through the native platform.
Volume-button shutter where permitted.
Headphone or external trigger support only where genuinely available.
Long exposure
Slower shutter speeds when supported.
Tripod warning.
Live exposure duration indicator.
Prevent the interface from promising long exposures on cameras that cannot perform them.
Flash
Off.
Auto.
On.
Torch.
Red-eye reduction where supported.
Only expose modes that the active camera hardware can actually provide.
7. Photo and Video Modes
Provide separate photo and video capture modes.
Photo mode
Live cinematic LUT preview.
Full-resolution still capture.
JPEG and HEIF when supported.
RAW/DNG where supported.
Timer and burst modes.
Manual exposure controls where available.
Metadata capture.
Original and graded file management.
Video mode
24, 25, 30, 50, and 60 FPS where supported.
Higher frame rates only when supported.
Available video resolutions.
Manual shutter and ISO where supported.
White balance lock.
Live LUT preview.
Video recording indicator and timer.
Audio recording permission and microphone status.
Recording controls and thermal/performance warnings where appropriate.
Use the 180-degree shutter guideline as an optional video recommendation, not a mandatory restriction.
For example:
24 FPS → approximately 1/48 or 1/50 second.
30 FPS → approximately 1/60 second.
60 FPS → approximately 1/120 or 1/125 second.
These are guidelines, not guarantees of available shutter speeds.
8. Interface Design
Create a premium dark interface inspired by understated Japanese camera design.
Visual direction
Deep charcoal backgrounds.
Warm off-white text.
Muted sage-green accents.
Thin dividers.
Small uppercase technical labels.
Minimal icons.
Carefully spaced controls.
Tactile rotary-dial interactions.
Subtle animations.
No unnecessary gradients, neon accents, or oversized cards.
Layout
The viewfinder must remain the main focus.
On mobile:
Full-screen camera preview.
Essential controls near the edges.
Quick-access exposure strip.
Swipeable LUT carousel.
Expandable Pro Mode drawer.
Large, accessible shutter button.
Thumb-friendly interactions.
On desktop:
Large central viewfinder.
Compact left-side camera controls.
Right-side Pro Mode panel.
Horizontal LUT carousel.
Keyboard shortcuts for shutter and common settings where appropriate.
Allow the user to enter a clean viewfinder mode with all nonessential overlays hidden.
9. Camera Settings and Persistence
Remember the user's preferences between sessions:
Last selected LUT.
LUT intensity.
Last camera-facing direction.
Preferred exposure mode.
Preferred white balance.
Grid settings.
Zebra and histogram settings.
Timer duration.
Preferred output format.
Preferred video settings.
Provide:
Restore defaults.
Save custom camera presets.
Rename presets.
Duplicate presets.
Delete presets.
Import/export configuration.
Do not persist camera permissions, fabricate device capabilities, or store private images on a remote server without explicit consent.
10. Capture Gallery
Implement a lightweight in-app gallery for images captured with the camera.
Include:
Thumbnail grid.
Capture date and time.
Selected LUT name.
Available exposure metadata.
Original and graded versions where supported.
Save to device.
Share through native sharing capabilities where supported.
Delete with confirmation.
Favorite captures.
Local-only storage by default.
Opening an image should lead to a simple capture review screen, not a full photo-editing interface.
11. Camera Capability Detection
This is a mandatory requirement.
When a camera is selected, query its real capabilities and configure the UI accordingly.
For each control, identify whether it is:
Fully supported by the hardware.
Partially supported.
Preview-only.
Unavailable.
Disable unavailable controls and explain why.
Do not silently substitute software effects for hardware controls.
Do not assume that all Android devices, iPhones, browsers, or external webcams expose the same capabilities.
In browser mode, use supported capabilities from the MediaStreamTrack API, such as getCapabilities(), getSettings(), and applyConstraints(), where applicable. Recognize that these APIs do not expose every professional camera control.
For advanced mobile capture, use native Android Camera2/CameraX or iOS AVFoundation implementations.
12. Performance and Reliability
Minimize camera preview latency.
Use GPU acceleration for LUT processing where possible.
Avoid unnecessary full-frame CPU processing.
Keep shutter response fast.
Release camera resources when leaving the camera screen.
Handle permission denial gracefully.
Handle cameras becoming unavailable.
Handle switching between front and rear cameras.
Handle unsupported settings without crashing.
Monitor thermal and memory constraints.
Avoid saving incomplete captures.
Maintain sensible fallback behavior on low-end devices.
13. Privacy and Security
Request camera and microphone permissions only when needed.
Process images locally by default.
Do not upload camera footage to a server.
Do not request unrelated permissions.
Explain when a feature requires native installation.
Keep saved captures under the user's control.
14. Deliverables
Build the actual application, not just a static mockup.
Deliver:
A working camera interface.
Live camera preview.
Functional shutter and capture review.
Working real-time LUT preview.
Pro Mode controls connected to actual supported hardware capabilities.
Camera capability detection and appropriate fallbacks.
Photo saving and gallery functionality.
Responsive mobile-first styling.
Reusable TypeScript components.
A documented setup and run process.
A feature compatibility matrix for web, Android, and iOS.
A clear list of implemented, partially implemented, and unavailable features.
Include unit tests for capability detection, LUT processing, and settings management, plus integration tests for the capture flow where possible.
15. Implementation Priorities
Develop the application in this order:
Phase 1 — Working camera
Camera permissions.
Live preview.
Front/rear switching.
Shutter and photo saving.
Responsive viewfinder.
Phase 2 — Cinematic camera
Live LUT preview.
Preset carousel.
LUT intensity.
Capture with the selected look.
Local gallery.
Phase 3 — Pro Mode
EV compensation.
ISO.
Shutter speed.
Focus.
White balance.
Lens and zoom.
Hardware capability detection.
Phase 4 — Advanced monitoring
Histogram.
Zebra.
Focus peaking.
Exposure meter.
Level indicator.
RAW support where available.
Phase 5 — Native professional camera
Android Camera2/CameraX.
iOS AVFoundation.
Advanced sensor controls.
Supported RAW and high-resolution capture.
Advanced video capture.
Do not mark a phase complete until its core functionality works on a real supported device.
Final Requirement
The result should feel like a minimalist professional cinematic camera that happens to have beautiful color science built in.
The user's primary workflow is:
Open camera → compose shot → select a film look → adjust exposure and focus → capture → save.
Never turn the main workflow into:
Upload photo → edit photo → export.
Prioritize camera functionality, image quality, real sensor controls, low latency, and a refined shooting experience over decorative features.
If the selected platform cannot support a requested feature, clearly document the limitation and implement the best honest fallback rather than faking the control.

# Kia Bongo Driving & Parking Game (Unity 2022.3 LTS or newer)

STATUS: code written and statically reviewed ONLY. It was NOT opened or compiled in Unity (no Unity in the authoring environment).
Expect to fix a few compile/runtime issues on first open.

## First run
1. Unity Hub > Add project from disk > this folder (2022.3 LTS recommended; Unity 6 also fine).
2. On first load the editor script auto-generates `Assets/Prefabs/KiaBongo.prefab`, `Assets/Scenes/MainMenu.unity` and `City.unity`
   (or run menu **Kia Bongo > Build ALL**). If Unity asks to switch Active Input Handling, accept (legacy Input is used).
3. Open `Assets/Scenes/MainMenu.unity`, press Play.
4. Windows build: **Kia Bongo > Build Windows 64-bit** -> `Builds/Windows/KiaBongo.exe`
   (batch: `Unity -batchmode -quit -projectPath . -executeMethod KiaBongoSetup.BuildAll` then `... -executeMethod KiaBongoSetup.BuildWindows64`).

## Controls
W/S gas+brake(reverse) · A/D steer · Space handbrake · I engine · R reset upright · M auto/manual · G/F shift · L lights · Z/X/C indicators/hazard · H horn
V camera (third person / cabin / hood) · RMB look · J job · K cancel job/parking · E interact · Esc pause · F1 help

## Layout
- `Scripts/Vehicle` controller, tuning, paint, lights, audio, **VehicleModelSlot** (modular visuals)
- `Scripts/Parking` ParkingSpot / ParkingLot / ParkingChallenge (10 levels, scored out of 1000)
- `Scripts/World` JobManager (cargo, missions, fuel, repair), WorldLocation, DayNightCycle
- `Scripts/Core` save (JSON file), settings, economy, garage UI, bootstrap
- `Scripts/Maps` data-driven MapRegistry (`Resources/maps.json`) + MapRoot
- `Scripts/UI` main menu, pause menu, HUD, minimap, cameras
- `Scripts/Editor/KiaBongoSetup.cs` generates prefab, city, scenes, build

## Real Kia Bongo model (integrated)
`Assets/VehicleModels/KiaBongo/` holds the supplied model, pre-processed for realtime use:
- original single mesh (1.5 M tris, 747 k verts) -> `body.obj` + 4 separate `wheel_N.obj` (so wheels spin), vertex-clustering decimated to ~330 k tris total
- scaled to metres (length ~4.7 m, front = +Z; width scaled x0.77 to fit the physics), textures resized 4096 -> 2048, metallic+roughness merged into one `kia_metal_smooth.png` (R=metallic, A=smoothness)
- `Kia Bongo > Build ALL` (or menu 4) builds `Resources/VehicleModel/KiaBongoReal.prefab`; `VehicleModelSlot` installs it at runtime, repositions the WheelColliders to the model's wheels, sets wheel radius, cockpit camera and paint renderer.
- Placeholder renderers are hidden but their colliders stay (collision). Delete the prefab to return to the placeholder truck.
Limitations: body is ONE mesh, so cab/cargo colours are one colour; placeholder indicator/brake-light cubes are hidden on the real model; no interior or opening doors; cockpit eye position is a guess (VehicleModelInfo.cockpitLocalPos); metallic/smoothness look may need tuning. Original 100 MB OBJ is not included.

## Adding maps (Iran expansion)
Build a scene with MapRoot + WorldLocation + ParkingLot, add to Build Settings, add an entry in `maps.json`. Tehran/Isfahan/Shiraz/Tabriz/Mashhad
are listed as "coming soon"; intercity routes are stored in the same file for future highway streaming.

## GitHub Actions cloud build (experimental)
A workflow is included at `.github/workflows/build-windows.yml`. It is a starting point for a cloud build, not a guarantee that this project compiles.
It requires a valid Unity Personal license configured as GitHub repository secrets (`UNITY_LICENSE`, `UNITY_EMAIL`, `UNITY_PASSWORD`) supported by the GameCI Unity Builder action. Never put license credentials directly in the workflow or commit them to the repository.
Run it from GitHub: Actions > Build Kia Bongo for Windows > Run workflow. If it fails, download the log/artifacts and inspect the first Unity compile error.

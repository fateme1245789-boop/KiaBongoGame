# Test report (honest status)

## Environment
Unity Editor: NOT available. C# compiler (dotnet/mono/mcs): NOT available. Network: offline.
=> The project has NEVER been compiled, opened, played or built. No EXE exists.

## What was actually done
- Manual line-by-line review of all scripts + scripted static check (duplicate classes, file/class name match, unknown component
  types, missing members on static classes, brace/paren balance): 0 findings. It cannot detect type errors or API misuse.
- Fixed: constant parasitic brake drag; null guards (wheels, lights, prefab); keyboard input ignored while paused
  (controller, lights, camera, HUD); Build ALL idempotent (never overwrites prefab/scenes, no duplicate city, build settings merged);
  batch-mode auto-setup conflict; settings apply spam; Unity 6 input-handler warning.
- Real Kia Bongo model integrated (see README). Mesh processing (wheel split, decimation, scaling, texture packing) was done offline in Python and checked visually on a 2D preview only; the Unity import, prefab build, material look, wheel spin and collider alignment are UNTESTED.
- Fixed in slot: placeholder colliders were being deactivated with the placeholder (truck would pass through buildings) - now only renderers are hidden.

## NOT tested (needs Unity)
Compile, Build ALL output, menu->city flow, driving/reverse/gears/brakes/wheels, cameras, jobs (load/deliver/pay), fuel, repair,
save/load, parking start/fail/retry/complete, pause/settings, Windows build.

# predictive-input-buffer

Trajectory-based input buffer (press + wave release) with a **touch interface** for Android.

## Touch controls

- **Floating "IB" button** (in level): tap it to cycle the stage `2/2 -> 4/4 -> 8/8 -> 16/8 -> OFF`
  (this replaces the **F6** key of the PC version). Drag it to move it anywhere.
- **IB button in the pause menu / main menu**: opens the full menu (stage selector,
  show/hide floating button, button opacity, logging).

## Stages (ticks = 1/240 s)

| Stage | All modes | Wave |
|-------|-----------|------|
| 1 | 2 ticks | 2 ticks |
| 2 | 4 ticks | 4 ticks |
| 3 | 8 ticks | 8 ticks |
| 4 | 16 ticks | 8 ticks |
| 5 | OFF | OFF |

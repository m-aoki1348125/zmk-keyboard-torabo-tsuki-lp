# Auto Mouse Layer Implementation

## Overview

This implementation adds an **Auto Mouse Layer** feature to the torabo-tsuki-lp keyboard. When you move the trackball, the keyboard automatically switches to a dedicated mouse layer with optimized bindings for mouse operations, then returns to the default layer after a timeout period.

## Features

### 🎯 Auto-Activation
- **Automatic layer switching**: Trackball movement triggers instant layer change
- **Smart timeout**: Returns to default layer after 1 second of inactivity
- **Idle detection**: Requires 50ms of prior idle time before activation

### 🖱️ Comprehensive Mouse Layer (Layer 1)
The mouse layer includes optimized bindings for efficient mouse operations:

#### Row 1 (Number Row)
- **Escape to default**: `&to 0` - Immediate return to default layer
- **Browser navigation**: Back, Forward, Refresh, Zoom In/Out
- **Right side**: Scroll up controls, volume controls, mute

#### Row 2 (QWERTY Row)  
- **Edit operations**: Undo, Cut, Copy, Paste, Redo
- **Scroll controls**: 4-directional scrolling (left, down, up, right)
- **System controls**: Application menu, Delete

#### Row 3 (ASDF Row)
- **Modifier keys**: Ctrl, GUI, Alt, Shift for extended functionality
- **Primary mouse**: Left/Right click optimally positioned
- **Central position**: Mouse clicks near trackball location
- **Advanced clicks**: Middle click, modifier combinations

#### Row 4 (ZXCV Row)
- **Layer toggle**: `&tog 1` - Manual mouse layer toggle
- **Advanced operations**: Complex undo/redo with modifiers
- **Mouse navigation**: Forward/Back buttons (MB4/MB5)
- **Layer access**: Momentary access to layers 2 and 3

#### Row 5 (Thumb Row)
- **Direct access**: Strategic mouse click placement
- **Mixed operations**: Combination of clicks and layer access

### ⌨️ Smart Combos
Enhanced combo system for quick operations:

- **Copy**: Keys 27+28 → Ctrl+C
- **Paste**: Keys 28+29 → Ctrl+V  
- **Undo**: Keys 26+27 → Ctrl+Z
- **Redo**: Keys 29+30 → Ctrl+Y
- **Mouse Toggle**: Keys 25+26 → Toggle mouse layer (from any layer)

### 🎨 Custom Behaviors
Advanced tap-dance and macro behaviors:

- **Multi-tap clicks**: Repeated tapping for multiple clicks
- **Smart scrolling**: Accelerated scroll with repeated taps
- **DPI adjustment**: Placeholder behaviors for sensitivity control

## Configuration Details

### Hardware Settings
```conf
# Mouse functionality
CONFIG_ZMK_MOUSE=y
CONFIG_ZMK_MOUSE_SMOOTH_SCROLLING=y

# Input processing
CONFIG_ZMK_INPUT=y
CONFIG_ZMK_INPUT_PROCESSOR_AUTO_ENABLE=y
```

### Auto-Layer Processor
```dts
trackball_auto_mouse_layer: trackball_auto_mouse_layer {
    compatible = "zmk,input-processor-auto-layer";
    layer = <1>;                    // Target mouse layer
    timeout-ms = <1000>;            // 1 second timeout
    require-prior-idle-ms = <50>;   // 50ms idle requirement
};
```

### Trackball Configuration
```dts
trackball_listener: trackball_listener {
    compatible = "zmk,input-listener";
    device = <&trackball>;
    xy-swap;                        // Swap X/Y axes if needed
    x-invert;                       // Invert X axis
    y-invert;                       // Invert Y axis
    input-processors = <&trackball_auto_mouse_layer>;
};
```

## Usage Guide

### Basic Usage
1. **Move the trackball** → Automatically switches to mouse layer
2. **Perform mouse operations** using the optimized key layout
3. **Stop moving** for 1 second → Returns to default layer
4. **Manual toggle** available via combo (keys 25+26) or `&tog 1`

### Pro Tips
- **Quick edits**: Use combos for instant copy/paste without thinking about layers
- **Smooth workflow**: The 50ms idle requirement prevents accidental layer switches during typing
- **Persistent mode**: Use `&tog 1` to stay in mouse layer for extended mouse work
- **Emergency exit**: Press escape (top-left) to immediately return to default layer

### Customization
You can adjust the following parameters in the devicetree:
- `timeout-ms`: How long to wait before returning to default layer
- `require-prior-idle-ms`: Minimum idle time before auto-activation
- `layer`: Which layer to activate (currently set to layer 1)

## Trackball Orientation
The implementation includes axis adjustments:
- `xy-swap`: Swaps X and Y movement axes
- `x-invert`: Inverts horizontal movement direction  
- `y-invert`: Inverts vertical movement direction

Adjust these settings in `torabo_tsuki_lp.dtsi` if trackball movement feels incorrect.

## Troubleshooting

### Layer doesn't activate
- Check that `CONFIG_ZMK_INPUT=y` is enabled
- Verify trackball hardware is working
- Ensure `require-prior-idle-ms` period has passed

### Layer activates too frequently
- Increase `require-prior-idle-ms` value
- Check for mechanical issues with trackball

### Timeout too short/long
- Adjust `timeout-ms` value in devicetree configuration
- Values between 500-3000ms are typically comfortable

### Wrong movement directions
- Adjust `xy-swap`, `x-invert`, `y-invert` settings
- Test with simple trackball movements to calibrate

## Build Instructions

1. **Standard build process**: Use existing GitHub Actions or local build
2. **No additional dependencies**: All features use standard ZMK capabilities
3. **Compatible with**: ZMK Studio for real-time keymap editing
4. **Firmware update**: Flash updated firmware to both keyboard halves

The implementation maintains full compatibility with existing functionality while adding seamless mouse layer automation.

## Technical Implementation

### Files Modified
- `torabo_tsuki_lp_left.conf` - Added mouse feature configurations
- `torabo_tsuki_lp_right.conf` - Added mouse feature configurations  
- `torabo_tsuki_lp.dtsi` - Added input processor and trackball listener
- `keymap.keymap` - Redesigned layer 1 as comprehensive mouse layer

### Architecture
The implementation uses ZMK's input processor system to monitor trackball events and automatically manage layer states, providing a seamless user experience without manual intervention.
# Enhanced Mouse Layer Implementation

## Overview

This implementation enhances the torabo-tsuki-lp keyboard with comprehensive mouse functionality. The keyboard includes a dedicated mouse layer (Layer 1) with optimized bindings for mouse operations, and a scroll-specialized layer (Layer 4) for intensive document navigation.

## Features

### 🖱️ Enhanced Mouse Support
- **Dedicated mouse layer**: Layer 1 optimized for mouse operations
- **Comprehensive controls**: Mouse clicks, scrolling, and navigation
- **Manual activation**: Toggle or momentary layer access

### 🖱️ Comprehensive Mouse Layer (Layer 1)
The mouse layer includes optimized bindings for efficient mouse operations:

#### Mouse Layer Row 1 (Number Row)
- **Escape to default**: `&to 0` - Immediate return to default layer
- **Browser navigation**: Back, Forward, Refresh, Zoom In/Out
- **Right side**: Scroll up controls, volume controls, scroll layer access (`&to 4`)

#### Mouse Layer Row 2 (QWERTY Row)  
- **Edit operations**: Undo, Cut, Copy, Paste, Redo
- **Scroll controls**: 4-directional scrolling (left, down, up, right)
- **System controls**: Application menu, Delete

#### Mouse Layer Row 3 (ASDF Row)
- **Modifier keys**: Ctrl, GUI, Alt, Shift for extended functionality
- **Primary mouse**: Left/Right click optimally positioned
- **Central position**: Mouse clicks near trackball location
- **Advanced clicks**: Middle click, modifier combinations

#### Mouse Layer Row 4 (ZXCV Row)
- **Layer toggle**: `&tog 1` - Manual mouse layer toggle
- **Advanced operations**: Complex undo/redo with modifiers
- **Mouse navigation**: Forward/Back buttons (MB4/MB5)
- **Layer access**: Momentary access to layers 2 and 3

#### Mouse Layer Row 5 (Thumb Row)
- **Direct access**: Strategic mouse click placement  
- **Scroll layer access**: `&mo 4` - Momentary access to scroll-specialized layer
- **Mixed operations**: Combination of clicks and layer access

### 🎯 Scroll-Specialized Layer (Layer 4)
A dedicated layer for intensive scrolling operations and browser navigation:

#### Scroll Layer Row 1 (Number Row)
- **Layer exit**: `&to 0` - Return to default layer
- **Web operations**: Zoom controls, refresh, reload
- **Intensive scrolling**: Multiple scroll up controls for fast navigation

#### Scroll Layer Row 2 (QWERTY Row)  
- **Browser tabs**: New tab, close tab, restore tab, reload, focus location, search
- **Directional scrolling**: Precision 4-directional scroll controls
- **Search operations**: Find in page (`Ctrl+F`)

#### Scroll Layer Row 3 (ASDF Row)
- **Modifier support**: Full modifier key support for complex operations
- **Scroll controls**: Direct scroll up/down access
- **Mouse integration**: Mouse clicks remain available during scroll operations

#### Scroll Layer Row 4 (ZXCV Row)
- **Layer management**: `&tog 4` - Toggle scroll layer persistence
- **Zoom operations**: Zoom in/out, reset zoom with keyboard shortcuts
- **Precision scrolling**: Enhanced left/right scroll capabilities
- **Layer bridge**: Access to other layers (2, 3) while in scroll mode

#### Scroll Layer Row 5 (Thumb Row)
- **Quick actions**: Essential scroll and click operations
- **Multi-modal**: Seamless integration of scroll and click operations

### ⌨️ Smart Combos
Enhanced combo system for quick operations:

#### Basic Edit Combos (Active in Mouse Layer)
- **Copy**: Keys 27+28 → Ctrl+C
- **Paste**: Keys 28+29 → Ctrl+V  
- **Undo**: Keys 26+27 → Ctrl+Z
- **Redo**: Keys 29+30 → Ctrl+Y

#### Layer Switching Combos (Multi-layer)
- **Mouse Toggle**: Keys 25+26 → Toggle mouse layer (from layers 0, 2, 3)
- **Scroll Toggle**: Keys 24+25 → Toggle scroll layer (from all layers)
- **Mouse to Scroll**: Keys 30+31 → Direct switch from mouse to scroll layer

#### Advanced Scroll Combos (Active in Scroll Layer)
- **Fast Scroll**: Keys 18+19 → Accelerated scroll up using tap-dance
- **Precision Zoom**: Keys 16+17 → Reset zoom to 100% (Ctrl+0)

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
```

### Trackball Configuration
```dts
trackball_listener: trackball_listener {
    compatible = "zmk,input-listener";
    device = <&trackball>;
    input-processors = <&trackball_auto_mouse_layer>;
};

// Trackball device configuration
trackball: trackball@0 {
    status = "okay";
    compatible = "pixart,paw3222";
    reg = <0>;
    spi-max-frequency = <2000000>;
    irq-gpios = <&gpio0 19 GPIO_ACTIVE_LOW>;
    power-gpios = <&gpio0 8 (GPIO_ACTIVE_HIGH | NRF_GPIO_DRIVE_H1)>;
};
```

## Usage Guide

### Basic Usage
1. **Activate mouse layer** → Use combo (keys 25+26) or `&tog 1` to switch to Layer 1
2. **Perform mouse operations** using the optimized key layout with trackball
3. **Return to default** → Press escape (top-left) or toggle off the layer
4. **Trackball support** → Use trackball for cursor movement in any layer

### Advanced Multi-Layer Usage
1. **Access scroll layer** from mouse layer → Press top-right key (`&to 4`) or thumb key (`&mo 4`)
2. **Intensive scrolling** in scroll layer → Optimized for document navigation and web browsing
3. **Layer switching combos** → Quick access between layers:
   - Keys 25+26: Toggle mouse layer from default layer
   - Keys 24+25: Toggle scroll layer from any layer
   - Keys 30+31: Direct switch from mouse to scroll layer  
4. **Persistent modes** → Use `&tog 1` for mouse layer or `&tog 4` for scroll layer

### Pro Tips
- **Quick edits**: Use combos for instant copy/paste operations in mouse layer
- **Smooth workflow**: Toggle layers as needed for different tasks
- **Persistent modes**: Use `&tog 1` for mouse layer or `&tog 4` for scroll layer during extended work
- **Emergency exit**: Press escape (top-left) in any layer to immediately return to default layer
- **Scroll optimization**: Use scroll layer for document reading, web browsing, or code navigation
- **Layer chaining**: Flow seamlessly between mouse layer → scroll layer → back to default
- **Browser mastery**: Scroll layer includes tab management, zoom controls, and navigation shortcuts
- **Combo efficiency**: Master the layer switching combos (25+26, 24+25, 30+31) for fastest workflow
- **Trackball anywhere**: Trackball works for cursor movement in any layer, not just mouse layer

### Customization
You can customize the following aspects:
- **Layer assignments**: Modify layer numbers for mouse (1) and scroll (4) layers in keymap
- **Key bindings**: Adjust mouse and scroll operations in respective layers
- **Combo positions**: Modify key positions for layer switching combos
- **Toggle vs momentary**: Change between `&tog` and `&mo` for different layer access patterns

## Trackball Orientation
The trackball uses default orientation as configured by the PAW3222 driver. If movement direction feels incorrect, axis inversion can be handled through software mapping or physical trackball ball placement adjustment.

For software-level axis adjustment, you may need to implement custom input processing or modify the keymap bindings to achieve the desired movement direction.

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
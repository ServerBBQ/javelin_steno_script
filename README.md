Script compiler for javelin-steno keymaps.

# Docs:

## RGB:
#### Set RGB  
`func setRgb(id, r, g, b)`  
For boards with RGB lights, sets an individual light to the specified red (`r`), green (`g`), and blue (`b`) values.  

#### Set HSV  
`func setHsv(id, h, s, v)`  
For boards with RGB lights, sets an individual light to the specified hue (`h`), saturation (`s`), and value (`v`).  

- **h** = hue, `0-65536` represents `0°` to `360°`.  
- **s** = saturation, `0-256` represents `0.0` to `1.0`.  
- **v** = value, `0-255` represents `0.0` to `1.0`.  

## Display:

#### Clear display:
`func clearDisplay(displayId)`

#### Set auto draw:
`func setAutoDraw(displayId, autoDrawId)`

**auto draw ids:**
```dart
none = 0
paper tape = 1
steno layout = 2
wpm = 3
```

#### Set draw color:
`func setDrawColor(displayId, color)`

#### Draw pixel
`func drawPixel(displayId, x, y)`
#### Draw line
`func drawLine(displayId, x1, y1, x2, y2)`
#### Draw rectangle
`func drawRect(displayId, left, top, right, bottom)`
#### Draw image
`func drawImage(displayId, x, y, image)`

An image can be uploaded by dragging and dropping an image to the javelin script editor

#### Draw text
`func drawText(displayId, x, y, fontId, alignment, text)`

# Other:

#### Release all  
`func releaseAll()` 

Releases all pressed scan codes and steno keys.  

#### Press all  
`func pressAll()`  

Calls all press scripts for buttons that are pressed.  

#### Check if in Press All  
`func isInPressAll()`

Returns non-zero if a `pressAll` is being processed.  

#### Check if a button is pressed  
`func isButtonPressed(buttonIndex)`

Returns `1` if the physical button is pressed.  

#### Check button state  
`func checkButtonState("01 10")`

Returns whether the current button state matches the string.  

- `0` = not pressed.  
- Space = ignore.  
- All others = pressed.  

Example:  
The string `"01 10"` checks that:  
- Button 0 is off.  
- Button 1 is on.  
- Button 3 is on.  
- Button 4 is off.  

The string should be the same length as the number of buttons.  

#### Send text  
`func sendText("Example")`  
Sends all the key presses required to emit the specified string.  

#### Get Time  
`func getTime()`
Returns the number of milliseconds since launch.  

#### Get LED Status  
`func getLedStatus(id)`  
Returns whether the LED status is on. See `LED_STATUS` constants.  

#### Adding options to the layout screen:  
`#option(<attributeName>, <option display category>, <option name>, <functionName>)`  
Adds an option to the list.

#### Adding options to the layout screen:
`#option(<attributeName>, <option display category>, <option name>, <functionName>)`

Adds a option to the list

`#dispatch(<local | per_layer>, <attributeName>, <option display category>, <defaultFunctionName>)`

Runs the function with the attribute
local means the option only shows in the layer the script is defined in, per_layer shows in all layers.

Usage example:
Here are multiple attributes with the same option display category. Using RGB Mode: Breathe updates two attributes simulatenously -> rgbUpdate goes to rgbUpdateKeyBreatheCycle, and rgbUpdateUnderglow -> noop
```dart
func rgbUpdate() {
  if (isBleAdvertising()) {
    rgbUpdateKeyConnectingCycle();
  } else if (isHostSleeping()) {
    rgbUpdateKeyBreatheCycle();
  } else {
    #dispatch(per_layer, rgbUpdate, "RGB Mode", rgbUpdateHueCycle);
  }
}

func rgbUpdateUnderglow() {
  if (isBleAdvertising()) {
    setUnderglowRgb(0, 0, 0);
  } else if (isHostSleeping()) {
    setUnderglowRgb(8, 8, 8);
  } else {
    #dispatch(per_layer, rgbUpdateUnderglow, "RGB Mode",  rgbUpdateUnderglowWhite);
  }
}

#option(rgbUpdateUnderglow, "RGB Mode",  "Random Colors", rgbUpdateUnderglowWhite)
func rgbUpdateUnderglowWhite() {
  setUnderglowRgb(64, 64, 64);
}

#option(rgbButtonPress, "RGB Mode", "Random Colors", rgbButtonPressRandomColor)
func rgbButtonPressRandomColor() {
  keyColorData[lastButtonIndex] = rand() & 0xffff;

  // If there's no update of keys in the tick loop, update it now.
  if (!isTimerActive(TIMER_ID_RGB_UPDATE)) {
    rgbUpdate();
  }
}
```

## USB scan codes

These constants are used with inbuilt functions:
 * func pressScanCode(SC_xxx)
 * func releaseScanCode(SC_xxx)
 * func tapScanCode(SC_xxx)
 * func isScanCodePressed(SC_xxx) var

```dart
const SC_NONE = 0;

const SC_A = 0x04;
const SC_B = 0x05;
const SC_C = 0x06;
const SC_D = 0x07;
const SC_E = 0x08;
const SC_F = 0x09;
const SC_G = 0x0a;
const SC_H = 0x0b;
const SC_I = 0x0c;
const SC_J = 0x0d;
const SC_K = 0x0e;
const SC_L = 0x0f;
const SC_M = 0x10;
const SC_N = 0x11;
const SC_O = 0x12;
const SC_P = 0x13;
const SC_Q = 0x14;
const SC_R = 0x15;
const SC_S = 0x16;
const SC_T = 0x17;
const SC_U = 0x18;
const SC_V = 0x19;
const SC_W = 0x1a;
const SC_X = 0x1b;
const SC_Y = 0x1c;
const SC_Z = 0x1d;

const SC_1 = 0x1e;
const SC_2 = 0x1f;
const SC_3 = 0x20;
const SC_4 = 0x21;
const SC_5 = 0x22;
const SC_6 = 0x23;
const SC_7 = 0x24;
const SC_8 = 0x25;
const SC_9 = 0x26;
const SC_0 = 0x27;

const SC_ENTER = 0x28;
const SC_ESC = 0x29;
const SC_BACKSPACE = 0x2a;
const SC_TAB = 0x2b;
const SC_SPACE = 0x2c;
const SC_MINUS = 0x2d;
const SC_EQUAL = 0x2e;
const SC_L_BRACKET = 0x2f;
const SC_R_BRACKET = 0x30;
const SC_BACKSLASH = 0x31;
const SC_HASH_TILDE = 0x32;
const SC_SEMICOLON = 0x33;
const SC_APOSTROPHE = 0x34;
const SC_GRAVE = 0x35;
const SC_COMMA = 0x36;
const SC_DOT = 0x37;
const SC_SLASH = 0x38;
const SC_CAPS = 0x39;

const SC_F1 = 0x3a;
const SC_F2 = 0x3b;
const SC_F3 = 0x3c;
const SC_F4 = 0x3d;
const SC_F5 = 0x3e;
const SC_F6 = 0x3f;
const SC_F7 = 0x40;
const SC_F8 = 0x41;
const SC_F9 = 0x42;
const SC_F10 = 0x43;
const SC_F11 = 0x44;
const SC_F12 = 0x45;

const SC_SYS_RQ = 0x46;
const SC_SCROLL_LOCK = 0x47;
const SC_PAUSE = 0x48;
const SC_INSERT = 0x49;
const SC_HOME = 0x4a;
const SC_PAGE_UP = 0x4b;
const SC_DELETE = 0x4c;
const SC_END = 0x4d;
const SC_PAGE_DOWN = 0x4e;
const SC_RIGHT = 0x4f;
const SC_LEFT = 0x50;
const SC_DOWN = 0x51;
const SC_UP = 0x52;

const SC_NUM_LOCK = 0x53;
const SC_KP_SLASH = 0x54;
const SC_KP_ASTERISK = 0x55;
const SC_KP_MINUS = 0x56;
const SC_KP_PLUS = 0x57;
const SC_KP_ENTER = 0x58;
const SC_KP_1 = 0x59;
const SC_KP_2 = 0x5a;
const SC_KP_3 = 0x5b;
const SC_KP_4 = 0x5c;
const SC_KP_5 = 0x5d;
const SC_KP_6 = 0x5e;
const SC_KP_7 = 0x5f;
const SC_KP_8 = 0x60;
const SC_KP_9 = 0x61;
const SC_KP_0 = 0x62;
const SC_KP_DOT = 0x63;

const SC_BACKSLASH_PIPE = 0x64;
const SC_COMPOSE = 0x65;
const SC_POWER = 0x66;
const SC_KP_EQUAL = 0x67;

const SC_F13 = 0x68;
const SC_F14 = 0x69;
const SC_F15 = 0x6a;
const SC_F16 = 0x6b;
const SC_F17 = 0x6c;
const SC_F18 = 0x6d;
const SC_F19 = 0x6e;
const SC_F20 = 0x6f;
const SC_F21 = 0x70;
const SC_F22 = 0x71;
const SC_F23 = 0x72;
const SC_F24 = 0x73;

const SC_OPEN = 0x74;
const SC_HELP = 0x75;
const SC_MENU = 0x76;
const SC_SELECT = 0x77;
const SC_STOP = 0x78;
const SC_AGAIN = 0x79;
const SC_UNDO = 0x7a;
const SC_CUT = 0x7b;
const SC_COPY = 0x7c;
const SC_PASTE = 0x7d;
const SC_FIND = 0x7e;
const SC_MUTE = 0x7f;
const SC_VOLUME_UP = 0x80;
const SC_VOLUME_DOWN = 0x81;
const SC_KP_COMMA = 0x85;

const SC_L_CTRL = 0xe0;
const SC_L_SHIFT = 0xe1;
const SC_L_ALT = 0xe2;
const SC_L_META = 0xe3;
const SC_R_CTRL = 0xe4;
const SC_R_SHIFT = 0xe5;
const SC_R_ALT = 0xe6;
const SC_R_META = 0xe7;

// From Consumer Page (0x0c)
const SC_CONSUMER_PLAY = 0xa0;                // UsageId 0xb0
const SC_CONSUMER_PAUSE = 0xa1;               // UsageId 0xb1
const SC_CONSUMER_RECORD = 0xa2;              // UsageId 0xb2
const SC_CONSUMER_FAST_FORWARD = 0xa3;        // UsageId 0xb3
const SC_CONSUMER_REWIND = 0xa4;              // UsageId 0xb4
const SC_CONSUMER_SCAN_NEXT_TRACK = 0xa5;     // UsageId 0xb5
const SC_CONSUMER_SCAN_PREVIOUS_TRACK = 0xa6; // UsageId 0xb6
const SC_CONSUMER_STOP = 0xa7;                // UsageId 0xb7
const SC_CONSUMER_EJECT = 0xa8;               // UsageId 0xb8

const SC_CONSUMER_STOP_EJECT = 0xbc;          // UsageId 0xcc
const SC_CONSUMER_PLAY_PAUSE = 0xbd;          // UsageId 0xcd
const SC_CONSUMER_PLAY_SKIP = 0xbe;           // UsageId 0xce

const SC_CONSUMER_MUTE = 0xd2;                // UsageId 0xe2
const SC_CONSUMER_VOLUME_UP = 0xd9;           // UsageId 0xe9
const SC_CONSUMER_VOLUME_DOWN = 0xda;         // UsageId 0xea
```

## javelin-steno steno key values.
These constants are used with inbuilt functions:
* func pressStenoKey(SK_xxx)
 * func releaseStenoKey(SK_xxx)
 * func isStenoKeyPressed(SK_xxx)

```dart
const SK_NONE = -1;
const SK_S1 = 0;
const SK_S2 = 1;
const SK_TL = 2;
const SK_KL = 3;
const SK_PL = 4;
const SK_WL = 5;
const SK_HL = 6;
const SK_RL = 7;
const SK_A = 8;
const SK_O = 9;
const SK_STAR1 = 10;
const SK_STAR2 = 11;
const SK_STAR3 = 12;
const SK_STAR4 = 13;
const SK_E = 14;
const SK_U = 15;
const SK_FR = 16;
const SK_RR = 17;
const SK_PR = 18;
const SK_BR = 19;
const SK_LR = 20;
const SK_GR = 21;
const SK_TR = 22;
const SK_SR = 23;
const SK_DR = 24;
const SK_ZR = 25;
const SK_NUM1 = 26;
const SK_NUM2 = 27;
const SK_NUM3 = 28;
const SK_NUM4 = 29;
const SK_NUM5 = 30;
const SK_NUM6 = 31;
const SK_NUM7 = 32;
const SK_NUM8 = 33;
const SK_NUM9 = 34;
const SK_NUM10 = 35;
const SK_NUM11 = 36;
const SK_NUM12 = 37;
const SK_FUNCTION = 38;
const SK_POWER = 39;
const SK_RES1 = 40;
const SK_RES2 = 41;
```

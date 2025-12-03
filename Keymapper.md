Here is a complete guide to all the special keys, layers, and chords in your configuration file 1.

This layout is built on two main ideas: **layers** (activated by holding `space` or `caps`) and **home-row modifiers** (tapping a key types a letter, holding it activates a modifier like `Ctrl` or `Alt`).

---

## 1. Main Layer Keys (How to Change Layers)

You have two primary keys that change your keyboard's function _while you hold them_:

- **`space` Key** 2
    
    - **Tap:** Types a normal `space`.
        
    - **Hold:** Activates the `symbols` layer.
        
- **`caps` Key** 3
    
    - **Tap:** Types `esc` (Escape).
        
    - **Hold:** Activates the `extend` layer.
        

---

## 2. Home-Row Modifiers (Tap vs. Hold)

Your home-row keys have dual functions. Tapping them (pressing and releasing quickly, under 200ms) types the letter. Holding them (over 400ms) turns them into modifier keys4.

|**Key**|**Tap (Normal)**|**Hold (Special)**|
|---|---|---|
|**`a`**|`a`||`lmet` (Left Windows/Cmd key) 5|
|**`s`**|`s`||`lalt` (Left Alt) 6|
|**`d`**|`d`||`lsft` (Left Shift) 7|
|**`f`**|`f`||`lctl` (Left Control) 8|
||||
|**`j`**|`j`||`rctl` (Right Control) 9|
|**`k`**|`k`||`rsft` (Right Shift) 10|
|**`l`**|`l`||`ralt` (Right Alt) 11|
|**`;`**|`;`||`rmet` (Right Windows/Cmd key) 12|

---

## 3. Layer Functions (What Happens When You Hold)

### `symbols` Layer (While Holding `space`)

This layer is for navigation and punctuation13.

- **Arrow Keys:**
    
    - `g` $\rightarrow$ Left Arrow 14
        
    - `h` $\rightarrow$ Down Arrow 15
        
    - `j` $\rightarrow$ Up Arrow 16
        
    - `k` $\rightarrow$ Right Arrow 17
        
- **Navigation:**
    
    - `n` $\rightarrow$ `home` 18
        
    - `m` $\rightarrow$ `pgdn` (Page Down) 19
        
    - `,` $\rightarrow$ `pgup` (Page Up) 20
        
    - `.` $\rightarrow$ `end` 21
        
- **Top Row (Numbers):** `! @ # $ % ^ & *` (by pressing `1` through `8`) 22
    
- **Bottom Row (Numbers):** `( ) { }` (by pressing `9`, `0`, `[`, `]`) 23
    

### `extend` Layer (While Holding `caps`)

This layer is for Function keys and your special "close window" chord24.

- **Function Keys:** Pressing `1` through `0`, `-`, and `=` will output `F1` through `F12`2525.
    
    - `4` $\rightarrow$ `F4` 2626
        
- **Close Window Chord:** The `q`, `w`, and `e` keys are armed for the close-window chord2727.
    
- **Other Keys:** All other keys (`_`) are "transparent," meaning they just pass through and type their normal `qwerty` letter28. For example, `(Hold caps) + r` just types `r`.
    

---

## 4. Chords (Pressing Keys Together)

You have two sets of chords (key combinations).

### **How to Close a Window**

This is the fix we added. It's activated on the `extend` layer.

- **Keystrokes:** **Hold `caps`**, then press **`q` + `w` + `e`** all at the same time292929.
    
- **Result:** `A-f4` (Alt+F4) is sent, closing your window30.
    

### **Right-Hand Chords**

These work on your main `qwerty` layer. You can press these key combinations at the same time (within 500ms) to get symbols31.

- `j` + `k` $\rightarrow$ `=` 32
    
- `k` + `l` $\rightarrow$ `+` 33
    
- `j` + `l` $\rightarrow$ `~` 34
    
- `l` + `;` $\rightarrow$ `-` 35
    
- `k` + `;` $\rightarrow$ `C-bspc` (Control+Backspace) 36
    

---

## 5. Other Important Remaps

- **Physical Left Alt Key:** Your physical `lalt` key (to the left of the spacebar) is **permanently remapped to `tab`** on your main `qwerty` layer37. To get `Alt`, you must **hold the `s` key**.
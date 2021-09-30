PS/2键盘驱动。

#### 模块

```
layouts	# 实现了各种键盘布局
	# Azerty - 法语的键盘布局
	# Dvorak104Key - 一种键盘布局
	# Jis109Key - 日语键盘布局
	# Uk105Key - 
	# Us104Key - 
```

#### 结构体

```rust
pub struct KeyEvent	{ 
	pub code: KeyCode,
	pub state:KeyState,
}

/* 封装解码和取样的逻辑，并处理状态转换和key事件。此数据结构占用10个字节 */
pub struct Keyboard<T, S> {/*omitted*/}

pub struct Modifiers {
    pub lshift: bool,
    pub rshift: bool,
    pub lctrl: bool,
    pub rctrl: bool,
    pub numlock: bool,
    pub capslock: bool,
    pub alt_gr: bool,
}

/* Scancode集合1的实现 */
pub struct ScancodeSet1;

/* Scancode集合2的实现 */
pub struct ScancodeSet2;
```

#### 枚举

```rust
pub enum DecodeState {
    Start,
    Extended,
    Release,
    ExtendedRelease,
}

pub enum DecodedKey {
    RawKey(KeyCode),
    Unicode(char),
}

/* 不同的错误情形 */
pub enum Error {
    BadStartBit,
    BadStopBit,
    ParityError,
    UnknownKeyCode,
    // some variants omitted
}

/* 对 Ctrl+字母 组合键的处理 */
pub enum HandleControl {
    MapLettersToUnicode,	// 把字母转换为Unicode字符
    Ignore,		// 不把字母转换为Unicode字符
}

/* Keycdoe是键盘生成的码 */
pub enum KeyCode {/* omitted */}

pub enum KeyState {
    Up,
    Down,
}
```

#### Traits

```rust
pub trait KeyboardLayout {
    // 把keycode转换为unicode字符
    fn map_keycode(
        keycode: KeyCode, 
        modifiers: &Modifiers, 
        handle_ctrl: HandleControl
    ) -> DecodedKey;
}

pub trait ScancodeSet {
    // 把scancode转换为key event
    fn advance_state(
        state: &mut DecodeState, 
        code: u8
    ) -> Result<Option<KeyEvent>, Error>;
    // 把scancode转换为keycode
    fn map_scancode(code: u8) -> Result<KeyCode, Error>;
    // 把scanncode扩展字节转换为keycode
    fn map_extended_scancode(code: u8) -> Result<KeyCode, Error>;
}
```




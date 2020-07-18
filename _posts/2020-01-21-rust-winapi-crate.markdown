---
layout: post
title:  "Rust winapi crate로 winapi 책 2장 띄우기"
date:   2020-01-21 00:00:00 +0000
categories: wasm rust winapi
comment: true
---

![legacy](/img/200121_winapi32.jpg)

무조건 외우라고 강조하시던 [추억의 WinAPI32 책 2장 코드](http://soen.kr/lecture/win32api/lec2/lec2-1-1.htm)를 Rust로 옮겼습니다. [winapi-rs](https://github.com/retep998/winapi-rs)를 사용합니다.

## 삽질 포인트
- 멀티바이트 지원 안 됨, 무조건 유니코드
  - https://github.com/retep998/winapi-rs/issues/845#issuecomment-562760350
  - 원인은 `MAKEINTRESOURCE` 이게 없음
 ```cpp
#define IS_INTRESOURCE(_r) ((((ULONG_PTR)(_r)) >> 16) == 0)
#define MAKEINTRESOURCEA(i) ((LPSTR)((ULONG_PTR)((WORD)(i))))
#define MAKEINTRESOURCEW(i) ((LPWSTR)((ULONG_PTR)((WORD)(i))))
#ifdef UNICODE
#define MAKEINTRESOURCE MAKEINTRESOURCEW
#else
#define MAKEINTRESOURCE MAKEINTRESOURCEA
#endif // !UNICODE
 ```
- 레퍼런스가 적어서 고생했는데 이거 참고했음
  - https://github.com/retep998/winapi-rs/issues/122
  - https://www.codeproject.com/Tips/1053658/Win-GUI-Programming-In-Rust-Language
  - 단 구버전이니까 `kernel32-sys`, `user32-sys`는 대체 할 수 있음, winapi crate [README](https://github.com/retep998/winapi-rs#should-i-still-use-those--sys-crates-such-as-kernel32-sys) 참조

## 코드
```toml
[package]
name = "simple_box"
version = "0.1.0"
authors = ["hyunjun529 <hyunjun529@gmail.com>"]
edition = "2018"

[target.'cfg(windows)'.dependencies]
winapi = { version = "0.3.8", features = ["winuser", "libloaderapi"] }
```

```rust
#[cfg(windows)] extern crate winapi;

use std::io::Error;
use std::mem::{size_of, zeroed};

#[cfg(windows)]
use winapi::shared::minwindef::{UINT, WPARAM, LPARAM, LRESULT, LPVOID, HINSTANCE};
use winapi::shared::windef::{HWND, HMENU, HBRUSH};
use winapi::um::winnt::{LPCSTR, LPCWSTR};
use winapi::um::winuser::{WNDCLASSEXW, LoadCursorW, LoadIconW, GetMessageW, DispatchMessageW, RegisterClassExW, CreateWindowExW, ShowWindow, MessageBoxA, TranslateMessage, DefWindowProcW, PostQuitMessage}; // functions
use winapi::um::winuser::{IDI_APPLICATION, IDC_ARROW, CS_HREDRAW, CS_VREDRAW, WS_EX_TOPMOST, WS_OVERLAPPEDWINDOW, CW_USEDEFAULT, WM_DESTROY, SW_SHOWDEFAULT}; // const variable
use winapi::um::libloaderapi::GetModuleHandleA;

#[cfg(windows)]
fn to_wstring(str : &str) -> Vec<u16> {
    use std::ffi::OsStr;
    use std::iter::once;
    use std::os::windows::ffi::OsStrExt;
    let v : Vec<u16> =
            OsStr::new(str).encode_wide().chain(once(0).into_iter()).collect();
    v
}

#[cfg(windows)]
unsafe extern "system"
fn wnd_proc(hwnd: HWND, msg: UINT, wparam: WPARAM, lparam: LPARAM) -> LRESULT {
    match msg {
        WM_DESTROY => {
            PostQuitMessage(0); 0
        },
        _ => {
            DefWindowProcW(hwnd, msg, wparam, lparam)
        }
    }
}

#[cfg(windows)]
fn print_message(msg: &str) -> Result<i32, Error> {
    let wide: Vec<u16> = to_wstring(msg);

    let ret = unsafe {
        let h_instance = GetModuleHandleA(0 as LPCSTR);

        let wndclass = WNDCLASSEXW {
            cbSize: size_of::<WNDCLASSEXW>() as u32,
            cbClsExtra: 0,
            cbWndExtra: 0,
            hbrBackground: 16 as HBRUSH,
            hCursor: LoadCursorW(0 as HINSTANCE, IDC_ARROW), // IDC_ARROW A가 없다, 무조건 W임
            hIcon: LoadIconW(0 as HINSTANCE, IDI_APPLICATION),
            hIconSm: LoadIconW(0 as HINSTANCE, IDI_APPLICATION),
            hInstance: h_instance,
            lpfnWndProc: Some(wnd_proc), 
            lpszClassName: wide.as_ptr(),
            lpszMenuName: 0 as LPCWSTR,
            style: CS_HREDRAW | CS_VREDRAW,
        };

        let window = CreateWindowExW(
            WS_EX_TOPMOST,
            wide.as_ptr(),
            wide.as_ptr(),
            WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            0 as HWND, 0 as HMENU,
            h_instance,
            0 as *mut winapi::ctypes::c_void
        );
        
        match RegisterClassExW(&wndclass) {
            0 => {
                MessageBoxA(
                    0 as HWND,
                    b"Call to RegisterClassEx failed!\0".as_ptr() as *const i8,
                    b"Win32 Guided Tour\0".as_ptr() as *const i8,
                    0 as UINT
                );
            },
            _atom => {
                let window = CreateWindowExW(
                    WS_EX_TOPMOST,
                    wide.as_ptr(),
                    wide.as_ptr(),
                    WS_OVERLAPPEDWINDOW,
                    CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
                    0 as HWND, 0 as HMENU,
                    h_instance,
                    0 as LPVOID
                );
                if window.is_null() {
                    MessageBoxA(
                        0 as HWND,
                        b"Call to CreateWindow failed!\0".as_ptr() as *const i8,
                        b"Win32 Guided Tour\0".as_ptr() as *const i8,
                        0 as UINT
                    );
                } else {
                    ShowWindow(window, SW_SHOWDEFAULT);
                    let mut msg = zeroed();
                    while GetMessageW(&mut msg, 0 as HWND, 0, 0) != 0 {
                        TranslateMessage(&msg);
                        DispatchMessageW(&msg);
                    }
                }
            }
        }
    };

    // if ret == 0 { Err(Error::last_os_error()) }
    // else { Ok(ret) }

    return Ok(0);
}

#[cfg(not(windows))]
fn print_message(msg: &str) -> Result<(), Error> {
    println!("{}", msg);
    Ok(())
}

fn main() {
    // https://docs.rs/winapi/0.3.0/winapi/um/winuser/index.html
    // https://github.com/retep998/winapi-rs/issues/122
    // https://www.codeproject.com/Tips/1053658/Win-GUI-Programming-In-Rust-Language
    print_message("Hello, world with rust winapi crate!").unwrap();
}
```

## 후기
- wasm 붙일려면 cfg로 매크로 분기 태워야 할 것 같다.
- 그걸 감안하고도 Rust는 갓언어가 맞음.
- VS Code 씁시다.

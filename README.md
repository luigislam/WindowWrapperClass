### WindowWrapperClass
Work in progress. It is "mostly" converting some Win Functions and code snippets into an Object.Property wrapper because I'm lazy.
Things you can do with this Class:
1. Easily get simple information about its size, position, if its hidden/active/minmax.
2. Easily know if your mouse is over the window or within a specific client area.
3. Much easier to manage a specific window since it uses the HWND and if that window stops existing then this class can find a new HWND from the original wintitle criteria used.
4. Restack windows so you can bring a window to the foreground without focusing or making the window active.
5. Has an OnExit callback for the Instance Object to unhide the window when the script closes.

### Example
```
If !WinExist("ahk_exe notepad.exe")
  Run "Notepad"
WinWait "ahk_exe notepad.exe"
NotepadWrapper := WindowWrapperClass("ahk_exe notepad.exe", True)
; Note: NotepadWrapper is saving whatever window's HWND it finds.
; Note2: the 2nd parameter being True will save the WinTitle and make "NotepadWrapper.isExist" automatically search for a new notepad window if it can no longer find the original hwnd.
NotepadWrapper.isAlwaysOnTop := True ; sets this Window to be always on top

Loop {
    If NotepadWrapper.Fn_isMouseOverClientRegion(0, 0, 100, 100)
        ToolTip "Mouse is over Client Area!",,, 2
    Sleep 1
}


#HotIf NotepadWrapper.isExist
F1:: NotepadWrapper.Fn_isAbove("a") ? NotepadWrapper.Fn_SetBelow("a") : NotepadWrapper.Fn_SetAbove("a") ; Re-orders the windows to place Notepad behind/over your current active window.
F2:: NotepadWrapper.isHidden := !NotepadWrapper.isHidden ; toggles to show/hide the Window.
F3:: NotepadWrapper.x := 200 ; moves the window's X position to these coordinates. You can also use the property to view its current x position.

#HotIf NotepadWrapper.isExist and NotepadWrapper.isMouseOver
RButton:: Tooltip "Right Clicked over Notepad window"

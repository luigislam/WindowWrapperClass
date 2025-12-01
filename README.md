# WindowWrapperClass
Work in progress. Just converting some Win Functions and code snippets into an Object.Property wrapper because I'm lazy.

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
    If NotepadWrapper.isExist
        Tooltip("Mouse is currently over the Notepad Window!")
    Else
        Tooltip
    Sleep 100
}

#HotIf NotepadWrapper.isExist
F1:: NotepadWrapper.x := 200 ; moves the window's X position to these coordinates. You can also use the property to view its current x position.
F2:: NotepadWrapper.isHidden := !NotepadWrapper.isHidden ; toggles to show/hide the Window.
F3:: NotepadWrapper.ControlSendKeys("Edit1", "LCtrl a") ; sends the key combination LCtrl and a to the Notepad window to select all text inside of it.
F4::{ ; Re-orders the windows to place Notepad behind/over your current active window.
    If NotepadWrapper.isOntop("A")
       NotepadWrapper.SetBelow("A")
    Else
        NotepadWrapper.SetAbove("A")
}

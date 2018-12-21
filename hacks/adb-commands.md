Find out the current running program:
adb shell dumpsys window windows | grep -E 'mFocusedApp'| cut -d / -f 1 | cut -d " " -f 7

Get Path:
adb shell pm path com.example.someapp


List Apps:
adb shell pm list packages

Start App:
adb shell monkey --pct-syskeys 0 -p '<appname>' -v 500

Watch Programm:
adb logcat | grep -F "`adb shell ps | grep com.example.endurancetest | cut -c10-15`"
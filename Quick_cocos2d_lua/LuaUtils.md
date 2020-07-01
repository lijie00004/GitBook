## Print

```lua
function printLog(tag, fmt, ...)
    local t = {
        "[",
        string.upper(tostring(tag)),
        "] ",
        string.format(tostring(fmt), ...)
    }
    print(table.concat(t))
end

printLog("INFO","%s [EventProtocol] dispatchEvent() - event %s", "111111", "eventName")
```


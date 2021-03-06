# BinaryFormatter

Simple command-line utility that reads in a binary file and writes out a
text file containing the hex representation of the source bytes.

A feature / benefit of BinaryFormatter is that it splits the hex string
across N lines of "ChunkSize" bytes each, with all lines prior to the
final line being appended with the T-SQL line-continuation character: `\`. This makes it easier to work with long hex strings (anything over 100k characters).

This was originally intended for automating builds of SQLCLR projects (i.e. convert the DLL into a hex bytes string for the `CREATE ASSEMBLY [name] FROM 0x\` statement) and for creating T-SQL scripts that create Certificates (i.e. convert the `.cer` file, and optionally the `.pvk` file, into a hex bytes strings for the `CREATE CERTIFICATE [name] FROM BINARY = 0x\` statement). However, it can be used for many other purposes. And, if needed, it would be easy enough to add an input parameter to allow for overriding the line-continuation character (e.g. `^` for DOS).

### Example

Source binary file = `0123456789ABCDEF`

Output file, using a _ChunkSize_ of `3`, contains:

```
012345\
6789AB\
CDEF
```

## Command Prompt / Automation Usage

`BinaryFormatter path\to\binary_file_name.ext [ path\to\OutputFile.sql ] [ ChunkSize ]`

* _ChunkSize_ = the number of bytes per row. A byte is 2 characters: 00 - FF.
* Maximum line length = (ChunkSize * 2) + 1.
* Default ChunkSize = 10000
* Default OutputFile = {path\\to\\binary\_file\_name}.sql

If `ChunkSize` is not supplied, the user is prompted to enter a value. A default value is shown inside square-brackets (e.g. `[ 10000 ]`), and hitting <kbd>Enter</kbd> without entering anything else will accept that default. Or, you can enter an integer value that is above zero. Any other value will cause the request to be repeated.


## Point-and-Click / "Send to" Usage

`BinaryFormatter.exe` can be used as a "Send to" destination so that you can easily convert binary files in Windows File Explorer simply by right-clicking on them, going to "Send to", and selecting **BinaryFormatter**.

To set this up, do the following:

1. Open File Explorer
1. Navigate to the `C:\Users\{your_windows_logon}\AppData\Roaming\Microsoft\Windows\SendTo` folder
1. Right-click in that folder, go to **New &gt;**, and select **Shortcut** (2nd from the top)
1. Click the **Browse...** button
1. Select `BinaryFormatter.exe` and click the **OK** button
1. Click the **Next** button
1. Change the name to: **Binary Formatter**
1. Click the **Finish** button
    <br><br>
  At this point you have a shortcut that will always prompt you to enter in a ChunkSize, or at least just hit <kbd>Enter</kbd> to accept the default value. If you want to convert files without being prompted to specify one, do the following:<br><br>
1. Right-click on the `Binary Formatter` shortcut and select **Properties**
1. Go to the **Shortcut** tab
1. In the **Target:** text field, add a number to the right of `...BinaryFormatter.exe`. I find that a value between 40 and 70 works best.
1. For the **Run:** drop-down, select **Minimized**
1. Click the **OK** button

And, you can even have two shortcuts, one with a specified ChuckSize, and one without :smile: .



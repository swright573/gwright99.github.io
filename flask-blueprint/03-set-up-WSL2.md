### Setting Up WSL2 on Windows
Setting up WSL2 on Windows is a relatively painless process. 
Microsoft has provided a helpful step-by-step guide [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10).<br><br>As per my Design Consideration #002 (don't recreate when
you can leverage someone to do it for you), I'll leverage the might of the Microsoft communications team to keep the steps fully up-to-date and identify important gotchas.

<br>With that said, I will not a few caveats that I encountered during my own upgrade effort:
<ul>
  <li>When I upgraded to WSL2 (mid-2020), joining the Windows Insider Program was mandatory to access the necessary build version 
  (I was following [this guide](https://char.gd/blog/2019/windows-web-dev-with-wsl2).
  I believe Microsoft recently changed this requirement, because its official guide does not mention this as a precursor step.</li>
  
  <br><li>While updating to Windows version 2004, my update process froze at 61%. This kicked off a multihour troubleshooting session 
  (<i>ultimately determined to be caused by a known Conexant sound driver conflict, and resolved by deleting everything in the C:\Windows\SoftwareDistribution folder</i>).
  <br><br>Hopefully this doesn't happen to you.</li>
  
  <br><li>Some WSL2 set up guides advise installing the new <a href="https://www.microsoft.com/en-ca/p/windows-terminal/9n0dx20hk701">Windows Terminal</a> after 
  completing the WSL2 install. I struggled (and failed) to configure the fonts & colours to an acceptable state, ultimately deciding to run my WSL2 in the native Windows launcher. <br>I likely am paying a performance price for having multiple WSL2 consoles open at once, but it hasnt't seemed to impact me yet. YMMV. </li>
</ul>
    

Previous: [Project Goals and Design Considerations](./02-project-goals-and-design-considerations.html)<br>
Next: 
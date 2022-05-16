
<div align = center>

# Worms

Source code for **Worms** by **David S. Maynard** from his original<br> floppies, generously released by him under the **MIT License**.

It is available for `Atari 8-bit` and `Commodore 64`.

<br>
    
---

**⸢ [Development Notes] ⸥ ⸢ [Scanned Source Code] ⸥**

---
    
<br>
<br>

##  Atari 8-bit

<kbd> 1983 / 04 / 19 </kbd>

<br>

<img
    src = 'Resources/Atari.jpg'
    align = right
    height = 270
/>

<br>

| Part | Forth<br>Source     | Atari<br>Image     
|:----:|:-------------------:|:-------------------:
| Ａ   | [📂][Forth Atari A] | [💾][Image Atari A] 
| Ｂ   | [📂][Forth Atari B] | [💾][Image Atari B] 

<br>

*The **FORTH** disk images are not bootable.*
    
</div>
    
<br>
<br>

### Instructions

1. Load **[ValForth]**

2. Switch to the source disk

3. Type 

    ```atari
    LIST <Screen Number>
    ```

<br>
<br>
<br>

<div align = center>

## Commodore 64

<br>

<img
    src = 'Resources/Commodore.jpg'
    align = right
    height = 240
/>

<br>

| Forth Source          | D64 Image     
|:---------------------:|:----------------------:
| [📂][Forth Commodore] | [💾][Image Commodore] 

<br>
    
*The **FORTH** disk image is not bootable and does* <br>
*not use a standard **Commodore** disk structure.*

</div>
    
<br>
<br>

### Details

`EAFORTH` & `SYSTEM` are fake files.

`EAFORTH` was probably a proprietary / custom<br>
version of the language used at **Electronics Arts**.

<br>


<!----------------------------------------------------------------------------->

[Scanned Source Code]: https://archive.org/details/worms-source-code
[Development Notes]: https://archive.org/details/david-maynard-worms-development-notes

[ValForth]: http://www.atarimania.com/utility-atari-400-800-xl-xe-valforth_17605.html


<!-------------------------------{ Source Code }------------------------------->

[Forth Commodore]: Source/Commodore/Worms.forth
[Forth Atari A]: Source/Atari/WormsA.forth
[Forth Atari B]: Source/Atari/WormsB.forth


<!------------------------------{ Disk Images }-------------------------------->

[Image Commodore]: ATR/Commodore.d64
[Image Atari A]: ATR/Atari-A.ATR
[Image Atari B]: ATR/Atari-B.ATR

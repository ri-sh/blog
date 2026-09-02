---
layout: post
title: "Visual Diff Tools in Linux"
date: 2014-05-21 10:39:00
tags: ["linux"]
thumbnail: /assets/img/migrated/visual-diff-tools-in-linux/img0.png
---

_Originally posted on my old blog on 2014-05-21._

Running the regular diff between two text files to see the differences is not so elegant for the human eye to decode. Luckily there are plenty of tools out there to make this easy.  
  
Command Line:  
  
sdiff file-1 file-2  
  
[![](/assets/img/migrated/visual-diff-tools-in-linux/img0.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiHOEe5gWBTv38vWFqqY8u5wniPIKpS1Vw2qtjbzW-FQKIwEk2qbwLybPwg4QvL7gwrWC46IziHUIuo8APqzyjjSmWjOSw9ltZURSAHJZt_67jWyN7DOumyZjqg6W35-8Qr2DmMzy4ky9we/s1600-h/sdiff.png)  
This is a much more elegant tool compared to diff, if you are looking for a quick command-line utility that shows the difference between two text files. While using it on big files, its better to pipe the output to less command.  
  
sdiff file1 file2 | less

  
~~Disadvantage - this is a read-only output. No editing or merging is possible. But its a great tool for a quick visual inspection.~~  


  
Update: Use sdiff -o out_file file1 file2 to interactively merge file1 and file2 and write the output into out_file. (Thanks to anonymous commenter)  
  
Vimdiff:   
  
vimdiff file-1 file-2  
  
[![](/assets/img/migrated/visual-diff-tools-in-linux/img1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjEXZJo0YYzTbmLBiTkov1Sp5JxIf6WnK8FQzc-3mF1WKbQgVY5buecS_pxtcb2-bF7FCvVCDOvrKQX_r_O8Nl8ukvN2XO12XF0r_J2B8Y082z2jQqgvfg9IEXFVoZKt6YTaNGYEVXbUpg-/s1600-h/vimdiff.png)  
This can open "n" number of files in a vertically split vim environment. This has color highlighting to specify the areas that differ in the file. Editing is possible. This is a complete vim-environment, so all the vim keys are usable. Here is a[quick and dirty tutorial](http://amjith.blogspot.com/2008/08/quick-and-dirty-vimdiff-tutorial.html) on how to use vimdiff.  
  
Emacs:  
  
M-x ediff-buffers  
[![](/assets/img/migrated/visual-diff-tools-in-linux/img2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKbyALJaDx5XgDSQD5CbPsjAHIfOsS9Yz1mTJiaALjp8lcc7Obm0F3lq_glZdjtYW6knaJJbSUKIau8EcIGMNtChkDLF_ocCdfLIdhaLdYYm2YDjEUcWEOx73hYAVmloj0lPUwbCUaFdbi/s1600-h/ediff.png)This is an emacs equivalent of vimdiff with copy to left, copy to right, merge changes and much more. This is a special ediff mode which has its own key bindings. Hit ? to get help on the keyboard shortcuts.  
  
Colored highlighting for distinguishing differences. Easy navigation to diff regions.  
  
A maximum of 3 files can be compared and merged. Both comand-line and gui mode are available.  
  
Visual Tools:  
  
[Meld:](http://meld.sourceforge.net/)  


[![](/assets/img/migrated/visual-diff-tools-in-linux/img3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLnn2YhgZD291GWF01bcL0iYVkdQ3A3bvCaFxZHuXo_35N514VmMkfXMX_LFoyJmaEZXf6w_AM9tNE6bE0kvdlnmPEGjAujYZvPzYDaIYPEfuLwMUILuaU6ZgS9nnbbdEQ7ZrAMO9Kyhaq/s1600-h/meld_preview.png)Image borrowed from Meld website

Can compare two or three files and allows editing. The differences are dynamically updated. This can work with version control systems like CVS, SVN etc. Folder comparison is possible.  
  
[Guiffy:](http://www.guiffy.com/)  


[![](/assets/img/migrated/visual-diff-tools-in-linux/img4.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhDoQpgYGL7ztotB4NFf1ETaKeFi8N4F2vEqhc9TyyjrXHWynTtmcup7-FoWcKiBDB5Q7QXizr3enBrP26tQPpgxasV4kxXy0Ls5d05emMeXPPqqU9_3qh1KyhhNcz_5nNrDZdZXCwU2gqS/s1600-h/Guiffy80Vista.jpg)Screenshot borrowed from Guiffy website.

  
  
Multi-platform visual diff and merge tool. Has a three-pane view for comparing two-files and the third pane to view the merged output file. Works in Window, Linux and Mac OS X. Folder comparison is possible.  
  
[kdiff3:](http://kdiff3.sourceforge.net/)  
  


[![](/assets/img/migrated/visual-diff-tools-in-linux/img5.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj37ZMEZ2qY9xoY1kaPIDsY8EwJDYSB1Roiv4Dn8-eu_LgJDND3ijoFqBR9cRFH-MUeJS3saZoyzzR7SYvx6kmsAkl_42Pzcru-Tnd6V_U5prRHyaGLloa4mTonq01v97fH3Lekl1pcaYsv/s1600-h/dirmergebig.png)Taken from the kdiff3's [website](http://kdiff3.sourceforge.net/).

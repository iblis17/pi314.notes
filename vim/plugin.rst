===================
= Vim Plugin 筆記 =
===================

vim 設定檔目錄結構
------------------

::

  ~/.vim/
  ├── bundle/
  ├── filetype.vim
  ├── ftplugin/
  ├── plugin/
  ├── syntax/
  └── vimrc

* ``bundle``

  - 包含你所安裝的 plugin

* ``filetype.vim``

  - 定義各種你需要的 filetype，例如 ::

      au BufNewFile,BufRead *.todo setf todo

    + 定義了一個 filetype ``todo``

* ``ftplugin/``

  - 定義每個 filetype 的 key map，例如 filetype ``todo`` 的 map 就放在 ``todo.vim`` 裡面

* ``plugin/``

  - 放置其他會被載入的 vim script

* ``syntax/``

  - 定義每個 filetype 的語法上色，例如 filetype ``todo`` 的語法上色就定義在 ``todo.vim`` 裡面

* ``vimrc``

  - 和 ``~/.vimrc`` 相同用途

自己寫 Plugin
-------------

現在已經有很成熟的套件管理系統可以使用，例如 Vundle_

..  _Vundle: https://github.com/gmarik/Vundle.vim

把你的 Plugin 設計成這樣的目錄結構:

::

  plugin-folder/
  ├── filetype.vim
  ├── autoload/
  │   └── pluginname.vim
  ├── ftplugin/
  │   └── pluginname.vim
  ├── syntax/
  │   └── pluginname.vim
  ├── README.rst
  └── LICENSE

* ``ftplugin/`` 裡面只放初始化以及 mappings
* ``autoload/`` 裡面放所有的邏輯

  - 內部使用的 function 用 ``s:func`` 命名，使其 scope 限定在該 script 內
  - 開放給使用者的 function 用 ``pluginname#func`` 命名，使其能夠被 autoload
  - **注意** ：autoload 並不是在所有狀況下都能使用 ::

      echom boshiamy#wide#type
      function! s:foo ()
          echom boshiamy#wide#type
      endfunction
      call s:foo()
      echom boshiamy#wide#type

    + 根據測試，上面這段 code snippet， ``s:foo()`` 內 **找不到** ``boshiamy#wide#type`` ，但外面（global scope）都可以
    + 目前不確定是 autoload 只對 function 而言有用，或是對 function 和變數的行為不一樣

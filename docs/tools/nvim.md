
## NERDTreeToggle
- 打开目录树
## 变量重命名
- rn：在vim命令模式下，变量重名。
## 代码格式化
- f：在命令模式下，对代码进行格式化。
## Gvimspector
- 生成debug文件
## debug
- vscode












<!-- 
 brew install neovim
 /Users/ss/.config/nvim
 coc
 brew install nodejs
 brew install yarn

Vimspector unavailable: Requires Vim compiled with +python3
pip3 install neovim

Make -DCMAKE_COMMPILE_COMMMANDS=1 -B .vscode ./

brew install ccls 
bear -- make -j4 -C .. 
sudo apt install nodejs
sudo npm install -g yarn

# ~/.vim/plugged/coc.nvim/是我的coc.nvim插件的安装目录
cd ~/.vim/plugged/coc.nvim/	
yarn install
yarn build

```
coc.nvim switched to custom popup menu from 0.0.82
Plug 'cateduo/vsdark.nvim'                                              │  you have to change key-mapping of <tab> to make it work. 
Plug 'jackguo380/vim-lsp-cxx-highlight'                                 │  checkout current key-mapping by ":verbose imap <tab>"
```
采用最近版本的配置
```
set signcolumn=number " 提示符放到行号上 (:h signcolumn)
inoremap <silent><expr> <TAB>
      \ coc#pum#visible() ? coc#pum#next(1) :
      \ CheckBackspace() ? "\<Tab>" :
      \ coc#refresh()
inoremap <expr><S-TAB> coc#pum#visible() ? coc#pum#prev(1) : "\<C-h>"

" Make <CR> to accept selected completion item or notify coc.nvim to format
" <C-g>u breaks current undo, please make your own choice.
inoremap <silent><expr> <CR> coc#pum#visible() ? coc#pum#confirm()
                              \: "\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"
```
 -->


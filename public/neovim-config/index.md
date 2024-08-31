# Neovim Config


# Neovim 配置文档

## 1. 插件管理

使用 `lazy.nvim` 进行插件管理。`lazy.nvim` 是一个高效的插件管理器，能够按需加载插件。

```
lua复制代码-- ~/.config/nvim/lua/config/lazy.lua
return {
  -- 插件列表
  {
    'nvim-treesitter/nvim-treesitter', -- Treesitter 插件
    run = ':TSUpdate',                 -- 安装后更新 Treesitter
  },
  {
    'neovim/nvim-lspconfig',           -- LSP 配置
    config = function()
      require'lspconfig'.pyright.setup{} -- Python LSP
    end,
  },
  -- 其他插件配置...
}
```

## 2. 配置文件结构

Neovim 的配置文件结构分为以下几个主要部分：

```
lua复制代码-- ~/.config/nvim/init.lua
-- 初始化配置
require('config.lazy') -- 插件管理
require('config.keymaps') -- 键位映射
require('config.settings') -- 常规设置
require('config.autocmds') -- 自动命令
```

## 3. 键位映射

定义了自定义的快捷键映射，以提高操作效率。以下是一个示例配置：

```
lua复制代码-- ~/.config/nvim/lua/config/keymaps.lua
vim.api.nvim_set_keymap('n', '<Leader>q', ':wq<CR>', { noremap = true, silent = true })
vim.api.nvim_set_keymap('n', '<Leader>s', ':w<CR>', { noremap = true, silent = true })

-- 将 y 键映射到 Ctrl+c
vim.api.nvim_set_keymap('n', 'y', '<C-c>', { noremap = true, silent = true })
```

## 4. 常规设置

设置 Neovim 的常规行为和界面选项，例如光标形状和行号显示。

```
lua复制代码-- ~/.config/nvim/lua/config/settings.lua
vim.o.number = true -- 显示行号
vim.o.relativenumber = true -- 相对行号
vim.o.cursorline = true -- 高亮当前行
vim.o.mouse = 'a' -- 启用鼠标支持

-- 光标样式
vim.o.guicursor = 'n-v-c:block,i-ci-ve:ver25,r-cr-o:hor20'
```

## 5. 自动命令

配置自动命令以在特定事件发生时执行自定义操作。例如，保存时自动修复文件：

```
lua复制代码-- ~/.config/nvim/lua/config/autocmds.lua
vim.api.nvim_create_autocmd("BufWritePre", {
  pattern = "*",
  command = "lua vim.lsp.buf.formatting_sync()",
})
```

## 6. 插件特定配置

为特定插件设置配置选项。例如，配置 `markdown-preview.nvim` 插件来在 WSL 中打开预览。

```
lua复制代码-- ~/.config/nvim/lua/config/markdown-preview.lua
return {
  "iamcco/markdown-preview.nvim",
  cmd = { "MarkdownPreviewToggle", "MarkdownPreview", "MarkdownPreviewStop" },
  ft = { "markdown" },
  build = function(plugin)
    if vim.fn.executable "npx" then
      vim.cmd("!cd " .. plugin.dir .. " && cd app && npx --yes yarn install")
    else
      vim.cmd [[Lazy load markdown-preview.nvim]]
      vim.fn["mkdp#util#install"]()
    end
  end,
  init = function()
    vim.g.mkdp_open_to_the_world = 1
    vim.g.mkdp_browser = 'wslview'
  end,
}
```

## 7. 主题和外观

配置 Neovim 的外观，包括主题设置和状态栏配置。

```
lua复制代码-- ~/.config/nvim/lua/config/theme.lua
vim.cmd [[colorscheme gruvbox]]
vim.o.background = "dark"

-- 状态栏配置
require('lualine').setup {
  options = { theme = 'gruvbox' }
}
```

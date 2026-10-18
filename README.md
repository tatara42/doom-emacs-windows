# Dependencies
winget install git
winget install -e --id BurntSushi.ripgrep.MSVC
winget install -e --id sharkdp.fd
winget install emacs

# Clone doom emacs
git clone https://github.com/hlissner/doom-emacs ~/.emacs.d

# Set environment variable
# user variable
HOME="C:\User\$USER"
# PATH
"$PATH:C:\Program Files\Emacs\emacs-31.1\bin"
"$PATH:C:\Users\$USER\.emacs.d\bin"

# Restart powershell
doom install

# Nerd Font
M-x nerd-icons-install-fonts
Install font

JetBrainsMono NF
(set-face-attribute 'default nil
                    :family "JetBrainsMono NF"
                    :height 120
                    :weight 'normal
                    :width 'normal)


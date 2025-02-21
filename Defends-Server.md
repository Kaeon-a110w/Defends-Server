### Defend-Server

## langkah awal :
- install iptables :
  `sudo apt update && sudo apt install iptables -y`
- blokir semua trafik masuk secara default
  `sudo iptables -P INPUT DROP`

## pindah port default ssh ke port baru yang kita tentukan untuk ssh server kita sendiri
- setting sshd_config
  `nano /etc/ssh/sshd_config`
- cari baris yang menggandung kata #port22 ubah menjadi port(yangditentukan) untuk ssh
contoh :
  `#port22 menjadi port1945`
lalu restart sshd
  `systemctl restart sshd`

## setting iptablesnya :
1. arahkan trafik dari port lama ke port baru
  `sudo iptables -t nat -A PREROUTING -p tcp 22 -j REDIRECT --to-port 1945`
2. izinkan trafik masuk pada port baru :
  `sudo iptables -A INPUT -p tcp --dport 1945 -j ACCEPT`
3. blokir port lama (opsional)
  `sudo iptables -A INPUT -p tcp --dport 22 -j DROP`
4. simpan aturan iptables
  `sudo iptables-save > /etc/iptables/rules.v4`

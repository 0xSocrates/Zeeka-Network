<h1 align="center"> Zeeka Network 

# Zeeka Network Chaos Testnet-1 kurulumu


## Sistem gereksinimleri (minimum):
```
2CPU, 4GB RAM, Ubuntu 20.04 işletim sistemi
```

## Sunucu güncellemesi ve gerekli kütüphanelerin kurulumu ile başlıyoruz

```
sudo apt-get update && sudo apt-get upgrade -y
```
```
sudo apt install build-essential libssl-dev cmake git screen -y
```

## Rust toolunu kuruyoruz:

 * Kurulum esnasında 1 - 2 ve 3 seçenekleri çıkacak, 1'i seçiyoruz.

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

![image](https://user-images.githubusercontent.com/101149671/196891892-ef3bb9b4-12f8-44fc-a062-7e008fa6c77a.png)
```
source "$HOME/.cargo/env"
```

## Bazukayı yüklüyoruz:
```
git clone https://github.com/zeeka-network/bazuka
```
Path'in kurulumu:
```
cd bazuka
```
```
cargo install --path .
```

## İnitalize işlemi
* Bu komutta `[SEEDMETNİ]` yazan yeri kendi belirlediğiniz herhangi bir metin ile değiştirip bunu saklamalısınız. Node taşıma işlemi yaparken bunu kullanacaksınız.
```
bazuka init --seed [SEEDMETNİ] --network chaos --node 127.0.0.1:8765
```

## Servis dosyası oluşturma:
* Bu komutta `[SUNUCU-IP]` ve `[DISCORD-ID]` yazan yerleri kendinize göre değiştirin.
* Tek seferde kopyalayıp not defterine yapıştırdıktan sonra `[SUNUCU-IP]` ve `[DISCORD-ID]` değiştirip yine tek seferde kopyalayıp terminale yapıştırarak bu işlemi tamamlayabilirsiniz
```
tee /etc/systemd/system/bazuka.service > /dev/null <<EOF
[Unit]
Description=Bazuka
After=network.target
[Service]
User=root
ExecStart=/root/.cargo/bin/bazuka node --listen 0.0.0.0:8765 --external  [SUNUCU-IP]:8765 --network chaos --db ~/.bazuka-chaos --bootstrap 152.228.155.120:8765 --discord-handle "[DISCORD-ID]"
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```
## Servisi başlatma

```
sudo systemctl daemon-reload
```
```
sudo systemctl enable bazuka
```
```
sudo systemctl start bazuka
```
# Kurulum tamamlandı

## Logları görüntülemek için:
```
journalctl -u bazuka -fo cat
```
## Node durumunu görmek için:
```
bazuka status
```
## Node durdurma:
```
systemctl stop bazuka
```
## Başlatma:
```
systemctl start bazuka
```
## Restart:
```
systemctl restart bazuka
```














# Sui Devnet Node Kurulum Rehberi
![github_cover](https://user-images.githubusercontent.com/102043225/184454358-0315e865-7066-427e-ac97-18e00dea1968.jpg)

|             |             |
| ----------- | ----------- |
| Yüksek verime, düşük gecikme süresine ve [Move programlama dili](https://github.com/MystenLabs/awesome-move) tarafından desteklenen varlık odaklı bir programlama modeline sahip yeni nesil akıllı sözleşme platformu olan Sui'nin Devnet Node kurulumuna hoş geldiniz! İhtiyacınız olan her şeyi [Sui Geliştirici Kılavuzlarında](doc/src/learn/index.md) bulabilirsiniz.      | <img src="doc/static/Sui_Icon_Brand.png" alt="sui_icon" width="200"/>      |
| Bu kurulum Docker ile yapılacaktır. | <img src="https://user-images.githubusercontent.com/102043225/184456263-90135109-4645-4305-8970-a5cd6fad53c9.png" alt="docker-logo" width="200"/>  |

# Sui Devnet Türkçe Kurulum Rehberi

## Sistemi Güncelleme
```shell
apt update && sudo apt upgrade -y
```

## Gerekli Kütüphanelerin Kurulması
```shell
apt install curl wget ca-certificates gnupg lsb-release make clang pkg-config libssl-dev build-essential git jq ncdu bsdmainutils htop -y < "/dev/null"
apt update && apt install jq -y
```

## Docker Kurulumu
```shell

curl -fsSL https://get.docker.com -o get-docker.sh 
sh get-docker.sh
```

## Docker Compose Yüklenmesi
```shell
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

## Docker Compose Yapılandırılması
```shell
cd $HOME && rm -rf sui
mkdir sui && cd sui
wget -q https://github.com/MystenLabs/sui/raw/main/crates/sui-config/data/fullnode-template.yaml
wget -q https://github.com/MystenLabs/sui-genesis/raw/main/devnet/genesis.blob
wget -q https://raw.githubusercontent.com/MystenLabs/sui/main/docker/fullnode/docker-compose.yaml
sed -i 's/127.0.0.1/0.0.0.0/' fullnode-template.yaml
```

## Uygulamayı Başlatma
```shell
docker-compose up -d
```


## Sui Discord Server'ına Mesaj Gönderilmesi
`#📋node-ip-application` kanalında node ip'nizi şu şekilde paylaşınız; `http://IP_ADRESINIZ:9000/`


## Explorer 
Node'u control etmek için bu [adrese](https://node.sui.zvalid.com/) giderek ip adresinizi giriniz.

## Cüzdan Oluşturma
### Yeni Cüzdan Alma
```shell
sui client new-address
```
`~/.sui/sui_config/` dizininde bulunan cüzdan anahtarı dosyalarınızı yedeklemeyi unutmayınız.

### Cüzdan Adresinizi Öğrenme
```shell
sui client active-address
```

### Token Alma
Sui Discord `#devnet-faucet` kanalında `!faucet CUZDAN_ADRESINIZ` şeklinde mesaj atarak token isteyiniz.

### Bakiye Kontrol Etme
`https://explorer.devnet.sui.io/addresses/CUZDAN_ADRESINIZ` şeklinde adresi tarayıcınıza girerek kontrol ediniz.


## Faydalı Komutlar

### Güncelleme
```shell
cd $HOME/sui
docker-compose down --volumes
rm genesis.blob
wget https://github.com/MystenLabs/sui-genesis/raw/main/devnet/genesis.blob
docker-compose up -d
```

### Loglar
```shell
docker-compose -f $HOME/sui/docker-compose.yaml logs -f --tail 10
```

### Node Silmek
```shell
rm -rf /usr/local/bin/{sui,sui-node,sui-faucet} 
cd $HOME/.sui && docker-compose down --volumes 
cd $HOME && rm -rf .sui
```

### Node Durumu Kontrol
```shell
service docker status
```

### Node'u Yeniden Başlatma
```shell
sudo systemctl restart suid
```

### Node'u Durdurma
```shell
sudo systemctl stop suid
```


# Guia de Instalação do LTSP

## Execução de Comandos como Root
Todos os comandos do terminal na documentação devem ser executados como root. Para isso, execute:

- No Ubuntu:
  ```sh
  sudo -i
  ```
- No Debian:
  ```sh
  su -
  ```

## Instalação do Sistema Operacional
O servidor LTSP pode ser headless, mas é recomendado instalar um sistema operacional com interface gráfica. O MATE recebe mais testes, mas qualquer ambiente desktop deve funcionar. Distribuições baseadas em `.deb` que utilizam systemd (Ubuntu Xenial, Debian Jessie ou mais recentes) são compatíveis. No Ubuntu, é recomendado remover o snap para evitar problemas.

## Adicionando o PPA do LTSP
O PPA do LTSP contém versões estáveis do LTSP. É obrigatório para distribuições anteriores a 2020 e recomendado para versões mais recentes. Consulte a página do PPA para adicioná-lo e continue com a instalação.

## Instalando os Pacotes do Servidor LTSP
Para transformar uma instalação comum em um servidor LTSP, execute:

```sh
apt install --install-recommends ltsp ltsp-binaries dnsmasq nfs-kernel-server openssh-server squashfs-tools ethtool net-tools epoptes
gpasswd -a administrador epoptes
```
Substitua `administrador` pelo nome do usuário administrador.
Se não estiver usando o PPA, substitua `ltsp-binaries` por `ipxe`.

## Configuração de Rede

### Servidor LTSP com NIC Única
Se o servidor LTSP possui uma única interface de rede e um servidor DHCP externo (roteador, pfSense, Windows Server), execute:
```sh
ltsp dnsmasq
```

### Servidor LTSP com Dual NIC
Se o servidor LTSP possui duas interfaces de rede, configure a NIC interna com IP estático `192.168.67.1` e execute:
```sh
ltsp dnsmasq --proxy-dhcp=0
```

## Manutenção da Imagem do Cliente
O LTSP suporta três métodos para manter a imagem do cliente:
1. **Chrootless**: Usa o sistema raiz do servidor como modelo.
2. **Imagem de VM**: Mantida graficamente, ex. via VirtualBox.
3. **Chroot**: Diretório separado mantido manualmente.

### Criando a Imagem do Cliente

- Para **chrootless**:
  ```sh
  ltsp image /
  ```
- Para **imagem de VM**, primeiro crie um link simbólico:
  ```sh
  ln -s "/home/user/VirtualBox VMs/debian/debian-flat.vmdk" /srv/ltsp/debian.img
  ltsp image debian
  ```
- Para **chroot em `/srv/ltsp/x86_32`**:
  ```sh
  ltsp image x86_32
  ```

Repita esses comandos sempre que instalar novos softwares ou atualizar a imagem.

## Configuração do iPXE
Depois de criar sua imagem inicial ou imagens adicionais, execute:
```sh
ltsp ipxe
```

## Configuração do Servidor NFS
Para configurar o servidor LTSP para servir imagens via NFS, execute:
```sh
ltsp nfs
```

## Gerando `ltsp.img`
Execute o seguinte comando para gerar `ltsp.img`:
```sh
ltsp initrd
```
Esse comando precisa ser executado após cada atualização do LTSP ou modificações no arquivo `/etc/ltsp/ltsp.conf`.

## Dúvidas?
Se tiver perguntas, participe das discussões ou acesse o chat online da comunidade LTSP.

Fonte: <https://ltsp.org/>

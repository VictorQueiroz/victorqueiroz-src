title: Configurando o Wi-Fi com uma Broadcom BCM4312 no Debian
tags:
  - Broadcom
  - Configuração
  - Debian
  - Instalação
  - Kernel
  - Linux
  - módulos
  - Placa de rede
  - Ubuntu
  - Wireless
id: 63
categories:
  - Debian
  - Linux
  - Sistemas Operacionais
  - Unix
date: 2014-09-28 07:07:40
---

O módulo que utilizaremos para fazer nossa placa Wi-Fi funcionar, será o **wl**, além da BCM 4312, o módulo suporta os seguintes dispositivos: BCM4311, BCM4313, BCM4321, BCM4322, BCM43224, BCM43225, BCM43227, BCM43228.

### Instalando e configurando o módulo

Primeiro você deve se autenticar como sudo:

<pre class="lang:sh decode:true ">su root</pre>

Caso não tenha adicionado os pacotes <span class="lang:js decode:true  crayon-inline ">contrib</span>  e <span class="lang:sh decode:true  crayon-inline ">non-free</span> aos seus espelhos. Faça isso! No caso, eu utilizo o Debian sid, então meu arquivo <span class="lang:js decode:true  crayon-inline ">/etc/apt/sources.list</span>  ficou assim:

<pre class="lang:sh decode:true "># 

deb http://ftp.br.debian.org/debian/ sid main contrib non-free</pre>

Agora atualize a lista de pacotes:

<pre class="lang:sh decode:true ">apt-get update</pre>

Instale o **module-assistant** e o **wireless-tools**:

<pre class="lang:sh decode:true ">apt-get install -y module-assistant wireless-tools</pre>

Compile e instale um dos pacotes broadcom-sta-modules-* para o seu sistema, utilizando o assistente de módulos (**module-assistant**):

<pre class="lang:sh decode:true ">m-a a-i broadcom-sta</pre>

Automaticamente, ele deve adicionar a lista negra do modprobe os pacotes que podem conflitar e acabar não permitindo que sua placa de Wi-Fi funcione corretamente, mas caso ele não adicione, vá até o seu arquivo <span class="lang:js decode:true  crayon-inline ">/etc/modprobe.d/broadcom-sta-common.conf </span> e cole todas as linhas abaixo:

<pre class="lang:sh decode:true"># wl module from Broadcom conflicts with ssb
# We must blacklist the following modules:
blacklist b43
blacklist b43legacy
blacklist b44
blacklist bcma
blacklist brcm80211
blacklist brcmsmac
blacklist ssb
install wl /sbin/modprobe --ignore-install wl $CMDLINE_OPTS</pre>

Assim, sempre que o seu sistema operacional for iniciado, ele forçará o carregamento do módulo **wl **e cancelará os outros módulos que conflitarão, caso tenham sido carregados.

Agora vamos atualizar o ramdisk, para que os arquivos que foram modificados sejam atualizados:

<pre class="lang:sh decode:true ">update-initramfs -u -k $(uname -r)</pre>

Mate os módulos que causarão conflitos e que podem estar sendo carregados:

<pre class="lang:sh decode:true">modprobe -r b43 b43legacy b44 bcma brcm80211 brcmsmac ssb</pre>

Inicie o módulo **wl**:

<pre class="lang:sh decode:true ">modprobe wl</pre>

### Configurando as interfaces de rede

Este é um passo que não deve ser ignorando, você deve abrir o arquivo <span class="lang:sh decode:true  crayon-inline">/etc/network/interfaces</span> e configurá-lo para que ele habilite a sua conexão de rede. Caso não saiba qual seja, digite <span class="lang:sh decode:true  crayon-inline ">ip addr show</span> e poderá ver, a placa de rede Wi-Fi sempre começa com **wlan **(i.e. wlan1, wlan2, wlan3...).

No meu caso, o meu arquivo <span class="lang:js decode:true  crayon-inline ">/etc/network/interfaces</span> está da seguinte maneira:

<pre class="lang:sh decode:true "># This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug eth0
iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0</pre>

Após terminar de configurar o seu arquivo, salve-o e recarregue-o:

<pre class="lang:sh decode:true ">/etc/init.d/networking restart</pre>

Ou:

<pre class="lang:sh decode:true ">service networking restart</pre>

Caso não realize nenhum destes procedimentos, sua placa Wi-Fi pode até se conectar a alguma rede através de gerenciadores de conexão pelo X, mas provavelmente não carregará nenhum site pelo navegador.

Para mais informações sobre como configurar a sua rede Wi-Fi pelo arquivo <span class="lang:sh decode:true  crayon-inline ">/etc/network/interfaces</span> , acesse [WiFi/HowToUse](https://wiki.debian.org/WiFi/HowToUse).

&nbsp;
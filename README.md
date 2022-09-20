# Kindlepi2
![](https://miro.medium.com/max/780/1*1TWFlSHPjbBe82wCDJLr0g.png)

https://medium.com/@vargascypher/kindlepi-2-3016ed511850

Hoje venho mostrar a vocês como utilizar o seu kindle 4 NT como monitor para sua [Raspberry pi](http://www.raspberrypi.org/).

Diferente de outros tutoriais neste não utilizaremos o VNC e nem emuladores de terminal como Kterm.

Inspirado e adaptado em Ponnuki, 2012. (utilizando kindle 3 e extensão de terminal **_myts_** )

[https://ponnuki.net/2012/09/kindleberry-pi/](https://ponnuki.net/2012/09/kindleberry-pi/)

## Vamos começar

Materiais utilizados para esse hack:

-   Kindle 4 NT( porém você pode utilizar qualquer kindle que possua um browser)
-   Raspberry Pi model 2 B (1GB de ram)
-   2 micro usb
-   Teclado

## Kindle Hack

```
IMPORTANTE: você pode brickar (tornar inutilizável) seu Kindle fazendo isso, eu não assumo responsabilidade pelo que você faz com seu Kindle, ou sua vida.
```

Atenção você já deve ter sua Raspberry Pi com seu SO já configurado, não irei abordar isso neste tutorial.

## JAIL BREAK (JB)

A primeira parte é realizar o processo de [Jail break no seu Kindle](http://wiki.mobileread.com/wiki/Kindle_Hacks_Information#Jail_break_JB), um processo que permite que os aparelhos executem aplicativos não autorizados pela fabricante.

**ATENÇÃO** : Para Kindle 4 utilizar este tutorial [aqui](https://wiki.mobileread.com/wiki/Kindle4NTHacking#Jailbreak) .

Para esse processo em outros kindle’s você deve realizar o download JB na versão mais atualizada, disponível [aqui](https://www.mobileread.com/forums/showthread.php?t=88004) na primeira página e siga o passo a passo para instalação disponível no fórum. Basicamente consistem em a você realizar o download, descompactar o arquivo, e com seu kindle plugado no PC, passar para o diretório raiz o arquivo **install.bin** com a versão do seu kindle. Após isto realizar o update do seu sistema.**\[HOME\] -> \[MENU\] > Settings -> \[MENU\] > Update Your Kindle**

## UsbNetwork

Para “**transmitirmos**” (irei explicar isso já já) o que ocorre com a Raspberry Pi para o Kindle, precisamos fazer com que o kindle seja reconhecido como um dispositivo de rede.

Precisaremos estão realizarmos a instalação de um pacote que nos permite fazer isso e também configura um servidor ssh em nosso dispositivo.

O [**UsbNetwork package**](https://www.mobileread.com/forums/showthread.php?t=88004) deve ser baixado pelo link, descompactado, e com seu kindle plugado, passar para o diretório raiz o arquivo **install.bin** com a versão do seu kindle. Após isto realizar o update do seu sistema. **\[HOME\] -> \[MENU\] > Settings -> \[MENU\] > Update Your Kindle**

> O arquivo de configuração pode ser encontrado no seu kindle em **src/usbnet/etc**

## Importante:

Windows e Linux usam quebras de linha diferentes e devem ser quebras do Linux. Por favor, use o [Notepad++](https://notepad-plus-plus.org/) no Windows exibindo os caracteres de controle para as quebras via “Exibir/Caracteres não imprimíveis/Mostrar fim de linha”. Tenha cuidado para não incluir quebras diferentes de — LF . Se algo der errado aqui, não vai funcionar! Você deve fazer os seguintes ajustes:

> K3\_WIFI=”verdadeiro”

Em vez de “false” para poder acessar não apenas via USB, mas também via WLAN via SSH.

> USE\_VOLUMD=”verdadeiro”

Deve ser o padrão, mas vale a pena mencionar, pois é recomendado para o Kindle 4.

Anote as especificações de IP para Kindle e host:

> HOST\_IP=192.168.15.201  
> KINDLE\_IP=192.168.15.244

Esses dados permanecem os mesmos — não importa a aparência de sua rede. Esses IPs são para acesso SSH via USB.

## Configurando o modo de depuração e a rede USB

Remova o Kindle do computador e digite ;debugOn no teclado virtual para ativar o modo de depuração e emitir comandos. Para ativar a rede USB, digite ~usbNetwork. Você pode desligar o modo de depuração com ;debugOff.

![](https://miro.medium.com/max/1340/1*zuvDEgc_tWvVImI4Ydf-dA.png)

**Obs**: Você pode facilitar isso instalando o _Kual (Launcher application)_

## Estabelecendo a conexão

Para testar sua conexão com o kindle é necessário alguns passos a mais, caso queira testar em windows siga este [link](https://www.tutonaut.de/anleitung-shh-zugriff-fuer-kindle-einrichten/) , um tutorial alemão porém que pode ser traduzido e seguido com facilidade. Se o Windows tiver problemas com a detecção automática e a instalação do driver, baixe [o arquivo do driver em mobileread.com](https://wiki.mobileread.com/wiki/NDIS_driver_setup_on_pc).

No linux e em sua **Raspberry Pi** você deve atribuir um ip para a nossa porta USB, adicione ao seu **/etc/network/interfaces** :

> allow-hotplug usb0
> 
> mapping hotplug  
> script grep  
> map usb0
> 
> iface usb0 inet static  
> address 192.168.15.2  
> netmask 255.255.255.0  
> broadcast 192.168.15.255  
> up iptables -I INPUT 1 -s 192.168.15.2 -j ACCEPT

Conecte o seu kindle na porta USB e tente realizar um ping para seu ip:

![](https://miro.medium.com/max/1400/1*spc_2-_1RREXYhEwQ8h3Wg.png)

Caso obtenha resposta tente acessar via SSH:

> ssh root@192.168.15.244

![](https://miro.medium.com/max/1058/1*W5rb6SkGoDPCz7V_CrTZrg.png)

A senha padrão depende do seu modelo e pode ser encontrada através da [ferramenta de senha raiz do Amazon Kindle](https://www.sven.de/kindle/#).

**Obs**: tente as 4 opções

![](https://miro.medium.com/max/1400/1*P32O46_U4KpjOdudpGKx0Q.png)

SUCESSO !!!!

**Obs**: Para testar o acesso de dentro da sua Raspberry Pi, acessa a Pi via ssh do seu computador local.

## Habilitar login automático em sua Raspberry Pi

Nós precisamos ter certeza que apenas um usuário ira se logar automaticamente e também que tenhamos uma sessão (_screen_) iniciada após boot.

Auto login tutorial ([link](https://cadernodelaboratorio.com.br/como-se-logar-automaticamente-no-raspberry/))

[https://cadernodelaboratorio.com.br/como-se-logar-automaticamente-no-raspberry/](https://cadernodelaboratorio.com.br/como-se-logar-automaticamente-no-raspberry/)

> O [GNU Screen](https://www.gnu.org/software/screen/) é um gerenciador de janelas de tela cheia que multiplexa um terminal físico entre vários processos, normalmente shells interativos.

No seu terminal crie uma sessão screen:

```
screen -S foobar
```

Edite seu profile

```
nano ~/.bash_profile or nano ~/.profile
```

Adicione as seguintes linhas:

```
if [[ -z "$STY" ]]; then   screen -xRR foobar # Nome da sua sessão criadafi
```

Aqui, usamos o sinalizador -x para anexar a uma sessão de tela não desanexada. E o sinalizador -RR tenta retomar a sessão de tela separada mais jovem (em termos de tempo de criação) encontrada.

Agora sempre que o login e realizado, ele é automaticamente direcionado para a sessão screen .

…

Certo conseguimos comunicar nossa Raspberry Pi com o kindle via ssh, porém como podemos utilizar ele para “**transmitir**” o que ocorre na Raspberry Pi ????

Como neste tutorial o foco é o acesso apenas ao **terminal** e não a interface gráfica da Raspberry Pi, precisamos de um jeito de acessa-la, algumas extensões de terminais como o (**_kterm_**) podem resolver isto porém o modelo que possuo Kindle 4 não é _touch screen_ e não possui teclado físico,dificultando o processo.

Para contornar isto utilizei a o browser disponível no modo experimental, para acessar minha Raspberry Pi via _web based SSH terminal,_ optei por utilizar o _(Shell In A Box)._

![](https://miro.medium.com/max/1400/0*6plAY5akTbt1DtuS.jpg)

Tutorial para instalar o _Shell In A Box em sua_ Raspberry Pi: [https://www.linuxhelp.com/how-to-install-shell-in-a-box-access-remote-linux-servers](https://www.linuxhelp.com/how-to-install-shell-in-a-box-access-remote-linux-servers)

Após as instalação desabilite o modo https do shell in a box alterando as configurações

```
nano /etc/default/shellinabox
```

Adicione “ — disable-ssl”

> SHELLINABOX\_ARGS="--no-beep --disable-ssl "

Agora com seu kindle devidamente configurado e conectado a Raspberry Pi. Acesse o browser do seu kindle e digite o ip da sua Raspberry Pi e a porta do servidor _Shell In A Box_ (padrão 4200).

Prontinho agora você tem seu mais novo monitor _E-ink !!!_

## Testes

https://medium.com/@vargascypher/kindlepi-2-3016ed511850

Obrigado por ler até aqui e bom hack !!!

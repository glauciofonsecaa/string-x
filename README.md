<center>

# STRX  - String X

Ferramenta de automatização rápida e personalizável desenvolvida para auxiliar analistas em diversas tarefas que dizem respeito à manipulação de strings em linhas de comando (linux).

[![Python 3.10](https://img.shields.io/badge/python-3.10-yellow.svg)](https://www.python.org/)
[![Build](https://img.shields.io/badge/Supported_OS-Linux-orange.svg)]()
[![Build](https://img.shields.io/badge/Supported_OS-Mac-orange.svg)]()

</center>

```
 + Autor:   MrCl0wn
 + Blog:    http://blog.mrcl0wn.com
 + GitHub:  https://github.com/osintbrazuca
 + GitHub:  https://github.com/MrCl0wnLab
 + Twitter: https://twitter.com/MrCl0wnLab
 + Email:   mrcl0wnlab\@\gmail.com
```


## FLUXO

A ferramenta reconhece fontes de dados, seja por meio do resultado de um cat, script scam ou request via curl. Os dados coletados da source são tratados como string e manipulados via parâmetro reservado da ferramenta. O loop de todo processo está diretamente relacionado aos dados coletados da fonte.


### STRING
É usada uma palavra chave ```{STRING}``` para receber os valores da fonte e concatenar com os comandos definidos pelo usuário. A keyword ```{STRING}``` é usada dentro do parâmetro reservado ```-str, -st```. A ferramenta faz o trabalo de subistiruir a palavra ```{STRING}``` pela string coletada da fonte. O intuito principal é **concatenação + execução de comando**.

Ex:
```bash
ARQUIVO
    host-01.com.br
    host-02.com.br
    host-03.com.br

COMANDO
    1 - cat hosts.txt | strx -st "host '{STRING}'"
    2 - strx -l hosts.txt -st "host '{STRING}'"

RESULTADO
    host 'host-01.com.br'
    host 'host-02.com.br'
    host 'host-03.com.br'

```
<br>

<center>

![Screenshot](/asset/fluxo.jpg)

</center>

---
## USO DA FERRAMENTA
``` 
--help / -h 
```
Isso exibirá ajuda para a ferramenta. Aqui estão todos os opções que ele suporta.

```bash
usage: strx [-h] [-list file] -str cmd [-out file] [-skip path] [-pipe cmd] [-verbose] [-thread <50>]

                                             _
                                            (T)          _
                                        _         .=.   (R)
                                       (S)   _   /\/(`)_         ▓
                                        ▒   /\/`\/ |\ 0`\      ░
                                        b   |░-.\_|_/.-||
                                        r   )/ |_____| \(    _
                            █               0  #/\ /\#  ░   (X)
                             ░                _| + o |_                ░
                             b         _     ((|, ^ ,|))               b
                             r        (1)     `||\_/||`                r  
                                               || _ ||      _
                                ▓              | \_/ ░     (V)
                                b          0.__.\   /.__.0   ░
                                r           `._  `"`  _.'           ▒
                                               ) ;  \ (             b
                                        ░    1'-' )/`'-1            r
                                                 0`     
                        
                              ██████    ▄▄▄█████▓    ██▀███     ▒██   ██▒ 
                            ▒██    ▒    ▓  ██▒ ▓▒   ▓██ ▒ ██▒   ░▒ █ █ ▒░
                            ░ ▓██▄      ▒ ▓██░ ▒░   ▓██ ░▄█ ▒   ░░  █   ░
                              ▒   ██▒   ░ ▓██▓ ░    ▒██▀▀█▄      ░ █ █ ▒ 
                            ▒██████▒▒     ▒██▒ ░    ░██▓ ▒██▒   ▒██▒ ▒██▒
                            ▒ ▒▓▒ ▒ ░     ▒ ░░      ░ ▒▓ ░▒▓░   ▒▒ ░ ░▓ ░
                            ░ ░▒  ░ ░       ░         ░▒ ░ ▒░   ░░   ░▒ ░
                            ░  ░  ░       ░           ░░   ░     ░    ░  
                                  ░                    ░         ░    ░  
                                  ░                                      
                                [+] https://github.com/osintbrazuca
                                [+] https://github.com/MrCl0wnLab
                                [+] by MrCl0wnLab

options:
             -h, --help             show this help message and exit
             -list file, -l file    Parâmetro arquivo com strings para excução
             -str cmd, -st cmd      String template da command shell
             -out file, -o file     Arquivo onde será salvo os valores
             -skip path, -s path    String que o processo vai pular. Ex: -s string ou --skip string
             -pipe cmd, -p cmd      Comando que será executado depois de um pipe |
             -verbose, -v           Modo verboso
             -thread <50>, -t <50>  Quantidade de threads

```

### EXEMPLO DE COMANDOS

Comandos que podem ser úteis para a criação de suas tricks.

```bash
python strx -l ./wordlist.txt -str 'curl -skL https://PHISHING/{STRING}/' -p 'grep -E "<title>(.*)</title>"' -v
python strx -l ./desaparecidos.txt -str 'grep -Ei "{STRING}" -R ./encontrados/' -v
python strx -l ./bins.txt -str 'grep -Ei "{STRING}" -R ./telegram/' -o resultado.txt -verbose
python strx -l ./cpfs.txt -str 'curl -skL "https://SERVER/api?={STRING}"' -out resultado.txt
python strx -l ./vermelho.txt -st 'grep -Ei "{STRING}" -R ./cores/' -out resultado.txt
python strx -l ./strings.txt -st 'grep -Ei "{STRING}" -R ./targets/' -pipe "awk -F ':' '{print \$2}'"
python strx -l ../rgs.txt -st './script --rg "{STRING}"' | grep 'SP:' | sort -u
```
### RECEBENDO STRINGS VIA PIPE
Pipe: Envia a saída de um comando para a entrada do próximo comando para continuidade do processamento. Os dados enviados são processados pelo próximo comando que mostrará o resultado do processamento. Pode ser usado desde um simples cat até mesmo resultados de algum fuzzing.

```bash
{PROCESSO}  | strx -st '{COMANDO} "{STRING}"'
```

```bash
cat pastas.txt | python strx -str 'rm -R {STRING}'
cat ips.txt    | python strx -str 'curl -s https://ipinfo.io/{STRING}/json}'
cat cpfs.txt   | python strx -str 'grep -Ei "{STRING}" -R ./leak/cpfs/'
cat apis .txt  | python strx -str './exploit --target {STRING}'
cat hosts.txt  | python strx -str 'host {STRING}'
curl -s 'https://ipinfo.io/8.8.8.8/json' --raw | jq -r  '.hostname' | python strx -st 'host {STRING}' -v
curl -s 'https://raw.githubusercontent.com/Zaczero/pihole-phishtank/main/hosts.txt' | python strx -str  'host {STRING}' -v
```

### USANDO PIPE PYTHON

É aceito o pipe via parâmetro da ferramenta, ele é executado usando o resultado do comando ```-strx / -st```.

> **Nota:** Não recomendo usar o ```|``` (pipe) diretamente nos parâmetros da ferramenta, uma vez que a lib python não tem suporte. ainda é possivel usar ```|``` (pipe) normalmente na saída.

```bash
-p '{COMMAND}' / -pipe '{COMMAND}'
```

```bash
cat host.txt  | python strx -str 'host {STRING}' -p 'grep -Eo "[[:digit:]]{1,3}\.[[:digit:]]{1,3}\.[[:digit:]]{1,3}\.[[:digit:]]{1,3}"'
cat sql.txt   | python strx -str 'curl -skL https://TARGET/{STRING}' -pipe 'grep "SQL syntax;"'
cat phish.txt | python strx -str 'python /tools/priv8.py -t "{STRING}"' -pipe 'grep "VULN"' | create_report
cat hosts.txt | python strx -str "curl -Iksw 'CODE:%{response_code};IP:%{remote_ip}' https://{STRING}"  -p "grep -o -E 'CODE:.(.*)|IP:.(.*)'" -t 30 
```

### TERMINAL  OUTPUT

-  Comando exemplo usado: ```cat hosts.txt  | python strx -str 'host {STRING}'```

![Screenshot](/asset/img1.png)

-  Comando exemplo usado: ```cat hosts.txt | python strx -str "curl -Iksw 'CODE:%{response_code};IP:%{remote_ip};HOST:%{url.host};SERVER:%header{server}' https://{STRING}"  -p "grep -o -E 'CODE:.(.*)|IP:.(.*)|HOST:.(.*)|SERVER:.(.*)'" -t 30``` 

![Screenshot](/asset/img3.png)

### VERBOSE
> usando -v / -verbose

-  Comando exemplo usado: ```cat hosts.txt  | python strx -str 'host {STRING}' -v```

![Screenshot](/asset/img2.png)

### ARQUIVO DE SAÍDA
> formato de arquivo output

```
output-%d-%m-%Y-%H.txt > output-15-06-2025-11.txt
```
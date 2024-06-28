# Experimental Attack (Website Footprinting)

> Este projeto consiste no desenvolvimento de um ataque cibernético.

# Índice
- [Introdução](#Introdução)
- [Desenvolvimento](#Desenvolvimento)
- [Conclusão](#Conclusão)

# Introdução

O ataque que desenvolvi será o Website Footprinting que é uma técnica utilizada para coletar informações sobre um site em específico. Essa coleta de informações pode fornecer informações confidenciais associadas ao site, como nomes e endereços registrados do proprietário do domínio, nomes de domínio, host dos sites, detalhes do sistema operativo, detalhes de IP entre outras informações.


# Desenvolvimento

Passos para realização deste ataque cibernético:

## 1. Utilizei a Amazon Web Services (AWS) para criar uma máquina com as seguintes especificações.
```sh
Nome: WindowsHost_Attacker
Sistema Operativo: Windows 11 Version 21H2
AMI: ami-0a5a29a3bda614760
Tipo de Máquina: m5.large
Tamanho de Disco: 64GB
Nome de Utilizador: ec2-user
Password: Passw0rd
```
## 2. Ainda na AWS realizei mais dois passos.
```sh
Criei um IP Elástico e associei-o à máquina que criei anteriormente. (52.73.83.30)
No grupo de segurança adicionei uma regra que permitia o acesso RDP da minha máquina à máquina criada.
```
## 3. Agora que já tinha todas as informações necessárias para conectar-me à máquina, utilizei o Remmina para aceder à mesma.

![remminaconnectionimage](https://github.com/rsr2004/experimentalattack/assets/145347631/d3417a02-994d-48e0-81f3-f7b0bad6d1e4)


## 4. Chegando até aqui, comecei por atualizar a máquina criada através do Windows Update.

![windowsupdateimage](https://github.com/rsr2004/experimentalattack/assets/145347631/790549a1-04ae-4eeb-b668-b9f51f0254b7)


## 5. Após a máquina estar devidamente atualizada fui para um prompt de comando e realizei o seguinte comando para encontrar o endereço IP do website:
```
ping www.rtp.pt
```

![pingresult1image](https://github.com/rsr2004/experimentalattack/assets/145347631/7ae2a481-4bff-4d63-b6bd-a07b11fb0dd7)

Com o resultado obtido pude observar que o endereço IP do domínio "rtp.pt" é 146.75.34.192. Ainda com este comando podemos obter informações sobre estatísticas de ping, como pacotes enviados, pacotes recebidos, pacotes perdidos e tempo aproximado de ida e volta.
NOTA: Este endereço pode diferenciar de máquina para máquina e dependendo se a RTP utiliza loadbalancers, por exemplo.

## 6. O comando que eu utilizei de seguida foi o seguinte, com o objetivo de descobrir o tamanho máximo do quadro.
```
ping www.rtp.pt -f -l 1500
```

![pingresult1500image](https://github.com/rsr2004/experimentalattack/assets/145347631/20663fec-c8b3-447b-bf09-9da5f5af98be)

A resposta, "Packet needs to be fragmented but DF set.", significa que o quadro é muito grande para estar na rede e precisa de ser fragmentado.
NOTA: O pacote não foi enviado porque usamos a opção -f com o comando ping, e o comando ping retornou este erro.

## 7. Após o sexto passo, o comando que eu utilizei foi o seguinte, ainda com o objetivo de descobrir o tamanho máximo do quadro.
```
ping www.rtp.pt -f -l 1300
```

![pingresult1300image](https://github.com/rsr2004/experimentalattack/assets/145347631/29976a43-b0e7-4ff6-9532-8f69a19bd6b3)

Com o resultado obtido após realizar este último comando ficamos a entender que o tamanho máximo do quadro está entre 1300 e 1500 bytes.

## 8. Para verificar, ao certo, qual o tamanho máximo do quadro temos que ir à tentativa até descobrir qual o tamanho máximo do quadro.
```
ping www.rtp.pt -f -l 1473
```
&
```
ping www.rtp.pt -f -l 1472
```

![pingresult1473image](https://github.com/rsr2004/experimentalattack/assets/145347631/f1d66064-2f06-41b2-aeb5-d381912af0e4)

![pingresult1472image](https://github.com/rsr2004/experimentalattack/assets/145347631/2c3aa3be-d73e-4509-b40e-4bac099a6695)

Estes resultados indicam que o tamanho máximo do quadro na rede desta máquina é de 1472 bytes.
NOTA: O tamanho máximo do quadro será diferente dependendo da rede de destino.

## 9. Agora, vamos descobrir o que acontece quando o TTL (Time to Live) expira. Cada quadro possui TTL definido. Se o TTL atingir 0, o router descarta o pacote.
```
ping www.rtp.pt -i 6
```
Esta opção define o TTL para 6.
NOTA: O valor máximo que podemos definir o TTL é 255.

![pingresultttl6image](https://github.com/rsr2004/experimentalattack/assets/145347631/75bf4ffd-c3c6-4114-a554-46e5016cb00b)

Este resultado significa que o router descartou o quadro porque o seu TTL expirou.

## 10. Após verificar o que acontece a um quadro quando o seu TTL expira, vou agora verificar qual o valor TTL até recebermos a resposta do host de destino.
```
ping www.rtp.pt -i 9 -n 1
```
&
```
ping www.rtp.pt -i 10 -n 1
```

![pingresultttlimage](https://github.com/rsr2004/experimentalattack/assets/145347631/03ea8e99-49ad-4b75-80fa-5ec38937ffdb)


Estes resultado implica que, no valor TTL de 27 a resposta é recebida do host de destino.


# Conclusão

Em suma, com este ataque de reconhecimento (Website Footprinting) consegui retirar algumas informações úteis para um ataque a esse mesmo website.

Algumas informações conseguidas:
- Endereço IP do domínio "rtp.pt": 146.75.34.192.
- Tamanho máximo do quadro: 1472 bytes.
- 
-
-
-
-
-
-

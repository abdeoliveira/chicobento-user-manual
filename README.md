`ver. 0.1`

# Cluster “Chico Bento”: Manual do Usuário

Prof. Dr. Alan Barros de Oliveira

Departamento de Física, Universidade Federal de Ouro Preto, Ouro Preto, MG, Ouro Preto, MG, Brasil. 35400-000.

29 de novembro de 2023


## Conexão

A conexão é feita via ssh através de terminal, com o comando `ssh -p PORT USERNAME@HOSTIP`.
Os dados de  `PORT`, `USERNAME` e `HOSTIP` são fornecidos diretamente ao usuário, justamente com a senha de acesso.

## Definições

O computador principal será denominado de `headnode` neste documento, enquanto os secundários serão chamados de `nodes`.
Estes últimos seguem o padrão de nomeclatura `cb01, cb02, ...`. Nos exemplos dados ao longo do texto, usamos `cb##` para denominar
um `node` genérico.

## Quota em disco

Todos os usuários iniciam com uma cota em disco inicial de `100 Gb` para o `/home`, com uma período de `7 dias` flexíveis (*grace period*) com ``150 Gb`. 
Pedidos justificados para aumento de quota podem ser feitos aos administradores do cluter.

## Submissão de jobs

Jobs devem ser submetidos diretamente nos `nodes`, sem sistema de filas no momento. 
Sugerimos os passos a seguir:

1. No `headnode`, use o commando `top-nodes` para visualizar o estado geral do cluster.
2. Conecte-se a um `node` livre, fazendo `ssh -p 1003 cb##`.
3. Execute seus programas.
4. Saia do `node` executando `exit` no terminal. Para conferir se seus jobs continuam rodando após logout do  `node`, execute `top-nodes cb##` 
no `headnode`.

## Reserva de `nodes`

Mediante solicitação justificada aos administradores do cluster, qualquer usuário pode pedir a reserva de `nodes` 
para si por um período de tempo determinado. Geralmente o pedido se justifica devido a prazos curtos para finalizações de 
artigos, teses, dissertações etc. Unidades reservadas aparecerão com status `LOCKED` (username)` , onde `username` 
é o nome de usuário a quem o `node` está reservado.

## Configurações avançadas (opcional)

### Conexão ssh sem senha entre `headnode` e `nodes`

É feita através do comando `connect-node`. Para configuração, siga os passos abaixo:

1. *No seu computador pessoal*, abra o arquivo `~/.bashrc` e, no final deste arquivo, adicione `export SSHPASS_A='parte_senha'`, onde `parte_senha` é uma parte da sua 
senha de acesso ao cluster (as aspas fazem parte do comando e devem ser incluídas). Digamos que sua senha completa seja `abcd1234`. 
Então você escreveria: `export SSHPASS_A='abcd'`. 
2. Conecte-se ao `headnode`.
3. No `headnode`, abra o arquivo `~/.bashrc` e adicione no final: `export SSHPASS_B='resto_senha'`. Seguindo nosso exemplo, você escreveria: `export SSHPASS_B='1234'`. 
4. Finalmente, ainda no `master`, execute `source ~/.bashrc`.

Você já deve ser capaz de se conectar aos `nodes` sem o prompt de senha, executando `connect-node cb##`. 

### LAMMPS com otimizações da Intel

No `Chico Bento` foi compilada uma versão otimizada para processadores Intel que, em testes preliminares, 
mostrou-se entre 20 e 30% mais rápida do que a versão sem otimização. Para utilizá-la, siga os passos abaixo:

1. Copie o executável otimizado do LAMMPS que encontra-se em `/home/public/lmp_intel_cpu_intelmpi` para sua pasta de trabalho.
2. Execute `source /opt/intel/oneapi/setvars.sh` para carregar as variáveis necessárias no sistema.
3. Ao executar o LAMMPS, inclua a flag `-sf intel` na linha de comando. Por exemplo: `mpirun -np 8 ./lmp_intel_cpu_intelmpi -sf intel -in input`. 

**OBS 1:** Caso se queira comparar esta versão otimizada com a versão "normal", esta última encontra-se em `/home/public/lmp_mpi`.

**OBS 2:** Mais detalhes sobre LAMMPS otimizado para processadores Intel podem ser obtidos em neste [link](https://docs.lammps.org/Speed_intel.html).




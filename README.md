# Cluster “Chico Bento”: Manual do Usuário

Prof. Dr. Alan Barros de Oliveira

Departamento de Física, Universidade Federal de Ouro Preto, Ouro Preto, MG, Ouro Preto, MG, Brasil. 35400-000.

## Conexão

A conexão é feita via ssh através de terminal, com o comando `ssh -p PORT USERNAME@HOSTIP`.
Os dados de  `PORT`, `USERNAME` e `HOSTIP` são fornecidos diretamente ao usuário, juntamente com a senha de acesso.

## Definições

O computador principal será denominado de `headnode` neste documento, enquanto os secundários serão chamados de `nodes`.
Estes últimos seguem o padrão de nomeclatura `cb01, cb02, ...`. Nos exemplos dados ao longo do texto, usamos `cb##` para denominar
um `node` genérico.

## Visão Geral

O comando `top-nodes` pode ser usado para se ter uma visão do estado do cluster.
O número de *cores* listado refere-se aos cores reais, 
desprezando os cores [Intel Hyper-Threading Technology](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html).

Os `nodes` `cb01` e `cb02` possuem 16 cores reais, 
mas 8 tem clock 4,90 GHz e 8 tem clock 3,60 GHz. Portanto, jobs submetidos com 16 cores serão processados com base nos cores mais lentos. 

## Quota em disco

Todos os usuários iniciam com uma cota em disco de `100 Gb` para o `/home`, 
com `50 Gb` *adicionais* por um período de `7 dias` (*grace period*). 
Pedidos justificados para aumento de quota podem ser feitos aos administradores do cluter.

## Submissão de jobs

Jobs devem ser submetidos diretamente nos `nodes`, sem sistema de filas no momento. 
Sugerimos os passos a seguir:

1. No `headnode`, use o commando `top-nodes` para visualizar o estado geral do cluster.
2. Conecte-se a um `node` livre, fazendo `ssh -p 1003 cb##`.
3. Execute seus programas.
4. Saia do `node` executando `exit` no terminal. Para conferir se seus jobs 
continuam rodando após logout do  `node`, execute `top-nodes cb##` no `headnode`.

## Reserva de `nodes`

Mediante solicitação justificada aos administradores do cluster, qualquer usuário 
pode pedir a reserva de `nodes` para si por um período de tempo determinado. 
Geralmente o pedido se justifica devido a prazos curtos para finalizações de 
artigos, teses, dissertações etc. Unidades reservadas aparecerão com status 
`LOCKED (username)` , onde `username` é o nome de usuário a quem o `node` está reservado.

## Backups

Não temos. Cada usuário é responsável pela segunrança dos seus dados.  

## Configurações avançadas (opcional)

### Conexão ssh sem senha entre `headnode` e `nodes`

Siga os passos abaixo:

1. No `headnode`, execute `ssh-keygen`.
2. Aperte `enter` para todas as perguntas (sem digitar nada).
3. Execute: `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`.
4. Crie o arquivo `~/.ssh/config` e escreva nele:

```
Host cb*
    Port 1003
    StrictHostKeyChecking=no
``` 

Você já deve ser capaz de se conectar aos `nodes` sem o prompt de senha, 
executando `ssh cb##`. 

### LAMMPS 

#### Sem otimizações 

Uma versão 'padrão' do LAMMPS está disponível em `/home/public/LAMMPS/lmp_g++_mpich`.

#### Com otimizações 

##### [Intel](https://docs.lammps.org/Speed_intel.html) 


Uma versão otimizada para processadores Intel
encontra-se em `/home/public/LAMMPS/lmp_intel_cpu_intelmpi`.

Para utilizá-la:

1. Execute `source /opt/intel/oneapi/setvars.sh` para carregar as variáveis 
necessárias no sistema.
2. Ao executar o LAMMPS, inclua a flag `-sf intel` na linha de comando. 
Por exemplo: `mpirun -np 8 ./lmp_intel_cpu_intelmpi -in input -sf intel`. 

##### [OpenMP](https://docs.lammps.org/Speed_omp.html) 

O pacote OpenMP foi criado por Axel Kohlmeyer na Universidade de Temple. 
O executável do LAMMPS com este pacote encontra-se em `/home/public/LAMMPS/lmp_omp`.

Para utilizá-lo:

1.  Ao executar o LAMMPS, inclua a flag `-sf omp` na linha de comando. 
Por exemplo: `mpirun -np 8 ./lmp_omp -in input -sf omp`. 
 
### SIESTA


#### Ver. 4.1, Intel MPI, MKL (Blacs e Scalapack)

Versão disponível em `/home/public/SIESTA/siesta_4.1_mpi_intel_mkl`.

Para utilizá-la: 

1. Execute `source /opt/intel/oneapi/setvars.sh` para carregar as
variáveis do sistema.

* Para execução em modo **paralelo**: 
`mpirun -np 4 ./siesta_4.1_mpi_intel_mkl < input > output`.
* Para execução em modo **serial**: `./siesta_4.1_mpi_intel_mkl < input > output`.



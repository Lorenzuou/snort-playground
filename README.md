# Projeto de Simulação de Ataque DDoS e Detecção com Snort usando Docker

Este projeto fornece um ambiente Docker para simular um ataque DDoS e demonstrar a detecção usando Snort. O ambiente consiste em dois containers Docker: um para o Snort (detector) e outro para simular o atacante.

O Snort é um sistema de detecção e prevenção de intrusões (IDS/IPS) de código aberto, amplamente utilizado para monitorar tráfego de rede em tempo real e detectar potenciais ameaças à segurança. Foi criado por Martin Roesch em 1998 e desde então tem sido uma ferramenta essencial para profissionais de segurança de rede.

## Estrutura do Projeto

```
snort-playground/
│
├── snort_conf/
│   ├── Dockerfile
│   ├── snort.conf
│   └── local.rules
│
├── attacker/
│   └── Dockerfile
│
└── README.md
```

## Pré-requisitos

- Docker


## Configuração

Todos os arquivos necessários já estão incluídos no repositório. Não é necessária nenhuma configuração adicional para começar.

## Execução

1. Clone este repositório:
   ```
   git clone https://github.com/seu-usuario/snort-playground.git
   cd snort-playground
   ```

2. Dentro da pasta snort-playground, crie a network snort-network

   ```
   docker network create snort-network

   ```
3. Construa o container do Snort e execute-o: 
   ```
   cd snort_conf
   docker build -t snort-container .
   docker run --name snort --network snort-network -it snort-container
   ```

4. Em um novo terminal, construa e execute o container do attacker: 

   ```
   cd attacker
   docker build -t attacker  .
   docker run --name attacker --network snort-network -it attacker
   ```

## Simulação do Ataque

1. Acesse o container do atacante (pule essa etapa caso já esteja dentro do container):
   ```
   docker compose exec attacker bash
   ```

2. No container do atacante, execute o ataque SYN Flood:
   ```
   hping3 -S --flood -V -p 80 snort

   ```

## Simulação de Ataques Adicionais

Além do ataque SYN Flood básico, você pode simular outros tipos de ataques para testar a detecção do Snort. 

1. **UDP Flood Attack**
   ```
   hping3 --udp -p 80 --flood snort
   ```

2. **ICMP Flood (Ping Flood) Attack**
   ```
   hping3 --icmp --flood snort
   ```

3. **Port Scanning** (usando nmap)
   ```
   nmap -p- -T4 snort
   ```

## Monitoramento da Detecção

1. Observe os alertas gerados pelo Snort em resposta ao ataque simulado.


## Personalização

- **Regras do Snort**: Modifique o arquivo `snort/local.rules` para ajustar ou adicionar regras de detecção.
- **Configuração do Snort**: Ajuste `snort/snort.conf` para modificar a configuração do Snort.
- **Atacante**: Se necessário, modifique `attacker/Dockerfile` para incluir ferramentas adicionais de simulação de ataque.


## Instalação do Snort 

Esses passos não devem ser replicados, eles são o passo a passo utilizado para criar os arquivos disponibilizados no repositório

1. Instalando o Snort em um sistema Linux:
   ```   
   sudo apt install snort
   ```

2. Configurando o arquivo principal do Snort (/etc/snort/snort.conf):
* Definindo a rede interna
* Ativando os módulos necessários
* Configurando as regras

3. Instalando as ferramentas para emulação de ataques:

* hping3: Uma ferramenta de linha de comando para gerar e analisar pacotes TCP/IP.
   ```
   sudo apt install hping3
    ```

* Low Orbit Ion Cannon (LOIC): Uma ferramenta de código aberto para testes de estresse em redes. 

   https://sourceforge.net/projects/loic/

   Importante notar de apenas usar em ambientes controlados e com permissão


* Tshark: Analisador de protocolo de rede para capturar tráfego. 
   ```
   sudo apt install tshark
   ```


4. Filtros no Snort para detecção de ataques DDoS:
   Foi criado um arquivo rules/ddos.rules com as regras para detectar diferentes tipos de ataques DDoS


## Fontes utilizadas

Para entender um pouco mais sobre o Snort: https://www.clubedolinux.com.br/implementando-ids-ips-com-snort-no-linux/

Tutorial de como instalar e aplicar o Snort: https://hackertarget.com/snort-tutorial-practical-examples/

Referência da documentação do Snort, especificamente de como as regras funcionam: https://docs.snort.org/start/rules

## Vídeo de Demonstração

https://drive.google.com/file/d/1bE_e-FwP98rgalhj6C0QzHhVL_me9KO5/view?usp=sharing
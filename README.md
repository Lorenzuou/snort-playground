# Projeto de Simulação de Ataque DDoS e Detecção com Snort usando Docker

Este projeto fornece um ambiente Docker para simular um ataque DDoS e demonstrar a detecção usando Snort. O ambiente consiste em dois containers Docker: um para o Snort (detector) e outro para simular o atacante.

## Estrutura do Projeto

```
snort-ddos-simulation/
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
   git clone https://github.com/seu-usuario/snort-ddos-simulation.git
   cd snort-ddos-simulation
   ```

2. Crie a network snort-network

   ```
   docker network create snort-network

   ```
3. Construa o container do Snort e execute-o: 
   ```
   cd snort_conf
   docker build -t snort-container .
   docker run --name snort --network snort-network -it snort-container
   ```

4. Construa e execute o container do attacker: 

   ```
   cd attacker
   docker build -t attacker  .
   docker run --name attacker --network snort-network -it attacker
   ```

## Simulação do Ataque

1. Acesse o container do atacante:
   ```
   docker-compose exec attacker bash
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

3. **HTTP Flood Attack** (usando siege)
   ```
   siege -c 200 -t 10S http://snort
   ```

4. **Port Scanning** (usando nmap)
   ```
   nmap -p- -T4 snort
   ```

7. **TCP Connection Flood**
   ```
   while true; do nc snort 80; done
   ```

8. **High Bandwidth UDP Flood** (usando iperf3)
   ```
   iperf3 -c snort -u -b 1G
   ```


## Monitoramento da Detecção

1. Observe os alertas gerados pelo Snort em resposta ao ataque simulado.


## Personalização

- **Regras do Snort**: Modifique o arquivo `snort/local.rules` para ajustar ou adicionar regras de detecção.
- **Configuração do Snort**: Ajuste `snort/snort.conf` para modificar a configuração do Snort.
- **Atacante**: Se necessário, modifique `attacker/Dockerfile` para incluir ferramentas adicionais de simulação de ataque.



# Cognos MQTT

Este projeto configura e executa um broker MQTT usando Docker Compose, simplificando a instalação e execução do serviço de mensagens MQTT. Abaixo, encontram-se instruções e uma explicação detalhada sobre a estrutura do Docker Compose e dos arquivos na pasta `mqtt`.

## Executando o MQTT com Docker Compose

Para iniciar o broker MQTT usando o Docker Compose, execute o seguinte comando:

```bash
docker-compose -f ./mqtt.docker-compose.yml up -d
```

Esse comando inicializa o broker em segundo plano (`-d`), conforme configurado no arquivo `mqtt.docker-compose.yml`.

### Estrutura do arquivo `mqtt.docker-compose.yml`

No arquivo `mqtt.docker-compose.yml`, o campo `version: "3"` indica a versão do Docker Compose utilizada. Essa versão define a sintaxe e os recursos compatíveis do Docker Compose. No caso, estamos utilizando a versão 3, que garante uma boa compatibilidade com versões modernas do Docker.

### Configuração dos serviços

No arquivo `mqtt.docker-compose.yml`, temos a configuração do serviço principal:

- **`services`**: Define os serviços que o Docker Compose gerenciará.
  - **`mosquitto`**: Serviço MQTT broker usando a imagem oficial do Eclipse Mosquitto.
    - **`image`**: Define a imagem a ser usada, neste caso `eclipse-mosquitto:2.0`.
    - **`container_name`**: Nome do container, aqui configurado como `mqtt-broker`.
    - **`restart`**: Define a política de reinício, sendo `unless-stopped` para que o container reinicie automaticamente, a menos que seja parado manualmente.
    - **`ports`**: Mapeia a porta do MQTT:
      - `"1883:1883"` mapeia a porta padrão MQTT para acesso externo.
    - **`volumes`**: Monta volumes para configuração e persistência de dados:
      - `./mqtt:/etc/mosquitto` monta o diretório de configuração `mqtt` do host para dentro do container.
      - `./mqtt/mqtt.conf:/mosquitto/config/mosquitto.conf` monta o arquivo de configuração `mqtt.conf`.

### Estrutura da pasta `mqtt`

- **`mqtt.conf`**: Arquivo de configuração principal do Mosquitto. Define parâmetros de funcionamento do broker, como portas, segurança e diretórios de dados.
- **`mqtt.conf.save`**: Um backup do arquivo `mqtt.conf`, que pode ser útil para recuperação em caso de alterações indevidas.
- **`passwd`**: Arquivo de senha para autenticação de usuários, caso o broker MQTT esteja configurado para exigir autenticação.

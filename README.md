
# Recuperação II - Prática

- Curso - Desenvolvimento de Sistemas
- Unidade Curricular - Internet das Coisas
- Docente - Gustavo Roberto de Souza

## Orientações Gerais
- A recuperação deverá ser realizada individualmente.
- Não é permitido o uso do celular durante a realização da atividade.
- Conceitos de Internet das Coisas (MQTT e Fluxos NodeRED)

## Passo-a-Passo (Clonar e Entrega)
1. Você deve fazer um fork desse repositório, na parte superior dessa página clique na botão de fork. 
2. Depois disso, você deve clonar o repositório para o seu computador, usando o seguinte comando.
   1. Selecione uma pasta no computador.
   2. Abra o CMD (Terminal).
   3. Execute o seguinte comando `git clone <url_do_repositório>`
3. Abra no seu VS Code a pasta do projeto.
4. Desenvolva os exercícios.
5. Ao finalizar você precisa comittar e enviar novamente para o github suas modificações.
   1. Primeiro precisamos adicionar as alterações ao stage, usando o comando  `git add .`.
   2.  Depois disso, você vai de fato commitar, usando o comando `git commit -m "sua mensagem"`.
   3.  Por fim, você precisa fazer push para o github, com o comando `git push origin master`.
6. Por fim, você deve copiar o link do seu repositório e fazer o envio no AVA. 
   1. Você deve adicionar como comentário na entrega do AVA.

## Situação Problema
Você como desenvolvedor de Softwares voltado para Internet das Coisas (IOT), ficou responsável por criar a comunicação de dados 
entre os dispositivos (Microcontroladores ESP32, Arduino, Demais "coisas") e uma base de dados, na qual os posteriormente será
utilizada para tomada de decisão da empresa. Hoje A fazenda DesiFarm implementou sensores de umidade do solo conectados a microcontroladores ESP32 espalhados pelo plantio. O sistema deve garantir que as plantas recebam água somente quando necessário, 
otimizando o uso dos recursos hídricos. Todos os dispositivos já estão configurados enviando e recebendo informações ao broker (MQTT).

  - Sempre que um dispositivo novo se conectar, deve se cadastrar, ou atualizar informações, numa collection chamada `devices` no Mongo; (2,00)
  - Os dispositivos enviam, via MQTT, o valor de umidade (%) a cada minuto:
    - Toda informação de umidade (%) deve ser armazenada numa tabela chamada `readings` dentro do PostgreSQL; (2,00)
    - Caso a umidade seja < 40%, deve registrar numa collection chamada `irrigation_alerts` do mongo a informação (2,00)
      - Se for < 40% deve enviar um comando para ligar a bomba de água; (1,00)
        - Só pode ligar bomba entre 05:00 e 09:00 e entre 17:00 e 21:00 (hora local). (1,00)
        - Fora desses horários, nunca ligar (mesmo que <40%). Registrar tentativa bloqueada numa tabela chamada `pump_alerts`. (1,00)
      - Se for >= 40% deve enviar um comando para deligar a bomba de água (Permitir qualquer horário); (1,00)


### Topics Sugeridos
- Sempre que o dispositivo se conectar;
  - `device/{id}/info`
- Envio frequente da umidade do dispositivo;
  - `device/{id}/status/humidity`
- Envio do comando para o ativar ou desativar bomba no dispositivo;
  - `device/{id}/cmd/pump`
  
### Objetos Sugeridos

#### Dados Devices
```js
{
  _id: UUID,
  device_id: "ESP32-001",
  model: "ESP32-001",
  firmware: "v1.0.0",
  ip_address: "192.168.0.1",
  location: "Sala de Armazenamento",
  created_at: "10-10-2025",
  updated_at: "10-10-2025",
  light: "on", // "on" ou "off"
  raw: "Dados com JSON.stringfy()"
}
```

#### Dados de Temperatura
```js
{
  device_id: "ESP32-01",
  humidity: 35,
  unit: "%"
  created_at: "2025-10-14T09:00:00Z"
}
```

#### Dados Comando ligar ou desligar bomba d'água
```js
{
  device_id: "ESP32-01",
  pump: "on"  // ou "off"
}
```

#### Dados que devem registrar quando rejeitar ligação da bomba
```js
{
  device_id: "ESP32-01",
  created_at: "10-10-2025",
  humidity: 35,
  unit: "%"
}
```
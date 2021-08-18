# Challenge Backend Developer
Desafio técnico para seleção de desenvolvedores backend para a Alume. 

## O que deve ser feito?
Você trabalha para uma empresa que faz reservas de espaços em foguetes para cargas e passageiros. Ela criou um acordo com a SpaceX e precisa criar uma página que liste os lançamentos previstos para os próximos 6 meses. **O seu papel, como desenvolvedor backend, é apenas criar um endpoint GET que retorne os dados desses lançamentos em NodeJS.**

O desafio pode ser feito em javascript puro, mas Typescript será um diferencial.
  
## Requisitos
### Criar um endpoint `GET /upcoming-launches`
Você pode criar um servidor NodeJS usando Express e organizar o projeto da forma como achar mais conveniente, seja MVC ou outro padrão. O endpoint não recebe nenhum parâmetro.

### Dentro do serviço, ler [o endpoint de lançamentos futuros da Space Devs](https://ll.thespacedevs.com/2.2.0/swagger#operations-launch-launch_upcoming_list)
O serviço deve carregar os dados do endpoint da Space Devs, filtrando por alguns parâmetros: `include_suborbital=true`, para que inclua lançamentos suborbitais e `lsp__name=spacex`, para que sejam recebidos apenas os lançamentos da SpaceX. Você pode usar quaisquer bibliotecas que tiver mais experiência para carregar os dados deste endpoint (Exemplo: Axios).

### Retornar um resumo dos lançamentos

Uma vez carregados os dados, deve-se **filtrar os lançamentos para apenas aqueles que acontecerão nos próximos 6 meses, baseado no parâmetro `window_start`** e **mapear cada lançamento para retornar um objeto no mesmo formato abaixo**:

```json
{
  "name": "Dragon CRS-2 SpX-23",
  "description": "SpaceX will launch the cargo variant of its Dragon 2 spacecraft on their 23rd commercial resupply services mission to the International Space Station.",
  "from": "Kennedy Space Center, FL, USA",
  "to": "Low Earth Orbit",
  "window_start": "15/09/2021"
}
```

As propriedades dos dados carregados que você usará no mapeamento são:

* **name**: Dentro do objeto `mission`, a propriedade `name` (`mission.name`);
* **description**: Dentro do objeto `mission`, a propriedade `description` (`mission.description`);
* **from**: Dentro do objeto `pad` e depois dentro do objeto `location`, a propriedade `name` (`pad.location.name`);
* **to**: Dentro do objeto `mission` e depois dentro do objeto `orbit`, a propriedade `name` (`mission.orbit.name`);
* **window_start**: No próprio objeto do lançamento, a propriedade `window_start` (`window_start`);

**Lembrando que a propriedade `window_start` deve estar no formato pt-BR**. Você deve usar [a feature de locale do Luxon para fazer isso](https://moment.github.io/luxon/#/formatting?id=intl-1).

Após o filtro e o mapeamento, o retorno final do nosso endpoint deverá estar no seguinte formato, com os lançamentos em uma variável `launches`:

```json
{
  "launches": [
    {
      "name": "Dragon CRS-2 SpX-23",
      "description": "SpaceX will launch the cargo variant of its Dragon 2 spacecraft on their 23rd commercial resupply services mission to the International Space Station.",
      "from": "Kennedy Space Center, FL, USA",
      "to": "Low Earth Orbit",
      "window_start": "15/09/2021"
    },
    {
      "name": "Inspiration4",
      "description": "Inspiration4 is the world’s first all-commercial astronaut mission to orbit. Jared Isaacman, founder and CEO of Shift4 Payments, is donating the three seats alongside him aboard Dragon to individuals from the general public.",
      "from": "Kennedy Space Center, FL, USA",
      "to": "Low Earth Orbit",
      "window_start": "28/08/2021"
    }
  ]
}
```

## Recursos a serem usados

### [ExpressJS](https://expressjs.com/)
Framework para criação de API's REST com NodeJS mais usada no mundo. 

### [Luxon](https://github.com/moment/luxon)
O Luxon é uma biblioteca feita pelo mesmo time do MomentJS. Depois da depreciação deste, Luxon se tornou a biblioteca recomendada para manipulação de data e hora.

### [Lauch Library 2.2](https://ll.thespacedevs.com/2.2.0/swagger#operations-launch-launch_upcoming_list)
API aberta e gratuita feita e atualizada por fãs de exploração espacial. Tem uma série de recursos sobre lançamentos, bases de lançamento, programas espaciais, etc. No nosso teste usaremos um único endpoint, `GET /launch/upcoming/`, [cuja documentação você pode acessar clicando aqui](https://ll.thespacedevs.com/2.2.0/swagger#operations-launch-launch_upcoming_list).

## Envio do teste

Você pode enviar o link para o seu repositório para developers@alume.com. Caso tenha dúvidas sobre algum aspecto do teste você pode enviá-las para o mesmo e-mail.

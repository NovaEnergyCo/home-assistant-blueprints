# Blueprints de Home Assistant · NovaEnergyCo

Planos (*blueprints*) de automatización para aulas y espacios inteligentes: encienden y apagan luces y aire
acondicionado según la ocupación, con sensores de puerta, de movimiento o contactos secos.

## ¿Cuál uso?

| Sensores del espacio | Plano | |
|---|---|---|
| Puerta + movimiento | [Aula inteligente - Puerta, movimiento y horario](aula-puerta-movimiento-horario) | [![Importar en Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FNovaEnergyCo%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Faula-puerta-movimiento-horario%2Faula_puerta_horario.yaml) |
| Solo movimiento o contacto seco, **con horario** | [Espacio inteligente - Movimiento y horario](espacio-movimiento-horario) | [![Importar en Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FNovaEnergyCo%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fespacio-movimiento-horario%2Fespacio_movimiento_horario.yaml) |
| Solo movimiento o contacto seco, **sin horario** | [Espacio inteligente - Movimiento](espacio-movimiento) | [![Importar en Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FNovaEnergyCo%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fespacio-movimiento%2Fespacio_movimiento.yaml) |

Todos usan los mismos cuatro ayudantes por espacio (`AUTO`, `ESTADO`, `TIMER`, `STATUS`) y los mismos
estados (`LIBRE`, `OCUPADO`, `PUERTA_ABIERTA`, `SCAN`), así que un espacio puede pasar de un plano a otro
sin crear ayudantes nuevos. Cada carpeta explica cómo funciona su plano.

## Otros planos de NovaEnergyCo

| Plano | Para qué |
|---|---|
| [Sincronizar interruptor con aire](https://github.com/NovaEnergyCo/sync_interruptor_aire) | Mantiene sincronizados un botón del interruptor y un aire (termostato o contactor) |
| [blueprint-ha-ai-sin-spv](https://github.com/NovaEnergyCo/blueprint-ha-ai-sin-spv) | Aire por horario laboral, movimiento y botón |

## Créditos

Los estados y ayudantes siguen el formato de
[Aula inteligente - Multi dispositivos](https://github.com/andersoncamero/blueprint_aulas) de andersoncamero,
para que sean compatibles.

## Licencia

[MIT](LICENSE) © NovaEnergyCo

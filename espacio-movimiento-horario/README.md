# Espacio inteligente - Movimiento y horario

[![Importar en Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FNovaEnergyCo%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fespacio-movimiento-horario%2Fespacio_movimiento_horario.yaml)

Para espacios **sin sensor de puerta**: arranca con el movimiento y sigue un horario. Funciona con sensores
de movimiento (`binary_sensor`) o con el **contacto seco** de un sensor de movimiento de 110 V (`switch`,
encendido = hay movimiento).

## Cómo funciona

| Evento | Qué hace |
|---|---|
| **Hora de inicio** | Solo en día hábil (Workday): pasa a **automático**. Si ya hay gente, enciende |
| **Movimiento** | `OCUPADO`. En automático enciende luces y aire a la temperatura de ocupación |
| **Sin movimiento** | `SCAN` con cuenta regresiva. Si nadie vuelve: apaga las luces y el aire se **apaga** o se **sube** a la temperatura de no ocupación. Queda `LIBRE` |
| **Movimiento durante el `SCAN`** | Cancela la cuenta y vuelve a `OCUPADO` |
| **Hora de fin** | Todos los días: pasa a **manual**. Si el espacio está vacío, apaga lo que quede (incluido el aire "subido") |
| **Fuera del horario (manual)** | Nunca enciende. Si alguien prendió a mano y se fue, tras el scan apaga |
| **`LIBRE` elegido a mano** | Apaga todo |

Luces y aire son opcionales: en un espacio con solo sensor, la automatización lleva el estado
(`OCUPADO`/`LIBRE`). El aire puede ser un termostato y/o un **contactor** (solo enciende y apaga; con
"subir temperatura" se apaga).

## Ayudantes que necesita

Créalos antes, uno por espacio (en *Ajustes → Dispositivos y servicios → Ayudantes*):

| Ayudante | Tipo | Configuración |
|---|---|---|
| `AUTO …` | Interruptor (`input_boolean`) | Modo automático |
| `ESTADO …` | Desplegable (`input_select`) | Opciones exactas: `LIBRE`, `OCUPADO`, `PUERTA_ABIERTA`, `SCAN` |
| `TIMER …` | Número (`input_number`) | Mínimo 0, **máximo 1800**, paso 1 |
| `STATUS …` | Texto (`input_text`) | Opcional: muestra el estado anterior |

> ⚠️ El `TIMER` debe aceptar al menos **1800**. El plano escribe ahí los segundos de la cuenta regresiva
> (p. ej. 420 de scan). Con un máximo de 100, Home Assistant rechaza el valor, la automatización se
> detiene y el espacio queda trabado en `SCAN` sin apagar.

Requiere Home Assistant 2024.10 o posterior.

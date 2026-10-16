# Espacio inteligente - Movimiento

[![Importar en Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FNovaEnergyCo%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Fespacio-movimiento%2Fespacio_movimiento.yaml)

Versión **sin horario** para espacios **sin sensor de puerta**: arranca con el movimiento.
Funciona con sensores de movimiento o con el contacto seco de un sensor de 110 V.

## Cómo funciona

| Evento | Qué hace |
|---|---|
| **Movimiento** | `OCUPADO`. Si el modo automático está encendido, enciende luces y aire (23 °C) |
| **Sin movimiento** | `SCAN` con cuenta regresiva. Si nadie vuelve: apaga el aire, espera y apaga las luces. Queda `LIBRE` |
| **Movimiento durante el `SCAN`** | Cancela la cuenta y vuelve a `OCUPADO` |
| **`LIBRE` elegido a mano** | Apaga todo |

Con el **modo automático apagado** no toca luces ni aire: solo actualiza el estado.
Luces y aire son opcionales.

Si necesitas horario, días hábiles o aire por contactor, usa
[Espacio inteligente - Movimiento y horario](../espacio-movimiento-horario).

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

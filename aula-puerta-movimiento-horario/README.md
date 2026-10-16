# Aula inteligente - Puerta, movimiento y horario

[![Importar en Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FNovaEnergyCo%2Fhome-assistant-blueprints%2Fblob%2Fmain%2Faula-puerta-movimiento-horario%2Faula_puerta_horario.yaml)

Para salones **con sensor de puerta**. La **puerta es el sensor principal** (entrar y salir) y el
**movimiento el secundario** (confirmar si quedó gente). Pensado para sensores de movimiento a batería,
que no detectan a alumnos quietos: una vez confirmada la ocupación, el salón queda ocupado hasta que se
vuelva a abrir la puerta.

## Cómo funciona

| Evento | Qué hace |
|---|---|
| **Se abre la puerta** | Si estaba `LIBRE` y en automático, enciende luces y aire. Estado `PUERTA_ABIERTA`. Si la puerta queda abierta el tiempo configurado **sin movimiento**, apaga |
| **Se cierra la puerta** | `SCAN`: espera movimiento durante el tiempo de scan. Si lo hay → `OCUPADO` hasta que se abra la puerta. Si no → apaga (o sube la temperatura) y queda `LIBRE` |
| **Movimiento estando `LIBRE`** | Por si la puerta no registró la entrada: en automático enciende y espera una segunda detección para confirmar; si no llega, apaga |
| **Protección** | `OCUPADO` sin ningún movimiento durante el tiempo máximo → apaga (0 = desactivada) |
| **Hora de inicio** | Solo en día hábil (Workday): pasa a **automático** (enciende y apaga). Si hay gente, enciende |
| **Hora de fin** | Todos los días: pasa a **manual** (solo apaga). Si el salón no está ocupado, apaga lo que quede |
| **`LIBRE` elegido a mano** | Apaga todo |

**Al quedar libre (dentro del horario):** *apagar el aire* o *subir a la temperatura de no ocupación*.
Fuera del horario siempre se apaga. El aire puede ser un termostato (`climate`) y/o un **contactor**
(`switch` de un control de carga); el contactor solo enciende y apaga.

## Entradas principales

Sensor de puerta, sensores de movimiento, luces (opcional), termostatos (opcional), contactores (opcional),
ayudantes (AUTO, ESTADO, TIMER, STATUS), sensor Workday (opcional), hora de inicio y de fin, modo al quedar
libre, temperatura de ocupación (23 °C) y de no ocupación (27 °C), tiempo de scan (420 s), tiempo con la
puerta abierta sin movimiento (300 s), tiempo máximo ocupado sin movimiento (60 min) y espera entre apagar
el aire y las luces (60 s).

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

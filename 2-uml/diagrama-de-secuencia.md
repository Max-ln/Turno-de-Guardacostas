# Diagrama de secuencia — Escenario principal: limpiar una mancha de hidrocarburo

Muestra el escenario principal del juego (RF-01, RF-03, RF-05, RF-06, RF-07, RF-08, RF-09): el jugador inicia la partida, un barco suelta hidrocarburo, el jugador elige la herramienta y la usa (bien o mal), y el nivel termina en victoria (ficha educativa) o derrota (reintentar).

```mermaid
sequenceDiagram
    actor J as Jugador
    participant UI as Interfaz (HUD)
    participant Ju as Juego
    participant N as Nivel
    participant B as Barco
    participant H as Hidrocarburo
    participant Her as Herramienta
    participant S as SaludDelMar
    participant F as Fauna

    J->>UI: Clic en "Jugar"
    UI->>Ju: iniciar()
    Ju->>N: iniciar()
    loop Mientras haya tiempo y salud > 0
        N->>B: moverse()
        B->>H: soltarResiduo()
        H-->>S: disminuir(cantidad) mientras no se limpie
        S-->>F: actualizar(salud)
        J->>UI: Selecciona herramienta (barrera)
        UI->>Her: seleccionarHerramienta("barrera")
        J->>UI: Clic sobre la mancha
        UI->>Her: usar(residuo)
        alt Herramienta correcta
            Her->>H: contener(8 s) / reducir(20 %)
            H-->>S: recuperar(cantidad)
            S-->>F: actualizar(salud)
            UI-->>J: Etiqueta "+Salud marina"
        else Herramienta incorrecta (ej. red)
            Her-->>Ju: Resta 3 s de tiempo
            UI-->>J: Mensaje explicativo de una frase
        end
    end
    alt Salud > 50 al terminar el tiempo
        N->>Ju: terminar() con victoria
        Ju-->>J: Muestra FichaEducativa
        J->>UI: Clic en "Continuar"
        UI->>Ju: avanzarNivel()
    else Salud = 0
        N->>Ju: terminar() con derrota
        Ju-->>J: Pantalla de derrota con "Reintentar"
    end
```

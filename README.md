# Chain of Responsibility

## Introducción

El patrón **Chain of Responsibility** es un patrón de diseño de comportamiento que permite pasar solicitudes a lo largo de una cadena de manejadores. Cada manejador decide si procesa la solicitud o si la pasa al siguiente manejador de la cadena. Este patrón se usa comúnmente para desacoplar el emisor de una solicitud de su receptor, proporcionando así flexibilidad en la asignación de responsabilidades.

## Ejemplo (Problema)

### Problema
Imagina que en un sistema de bases de datos de una institución educativa, como el **Instituto Tecnológico de Tijuana**, se requiere procesar solicitudes de verificación de créditos de tutoría para estudiantes de diferentes semestres. El sistema necesita verificar los créditos de acuerdo con diferentes reglas, como el número de créditos obtenidos por semestre y el tipo de actividad realizada. ¿Cómo podemos organizar estas verificaciones para que no estén acopladas a una sola clase o función?

## Solución

El patrón **Chain of Responsibility** permite dividir el proceso de verificación en diferentes manejadores independientes. Cada manejador puede verificar un aspecto específico de la solicitud, y si no es capaz de procesarla, la pasa al siguiente manejador en la cadena.

Al usar este patrón, cada regla de validación se convierte en un manejador autónomo, permitiendo añadir o eliminar reglas sin afectar a las demás. Esto es especialmente útil en sistemas de verificación de datos donde las reglas pueden cambiar o ser extensibles.

## Objetivos

- Desacoplar el emisor de solicitudes de los receptores.
- Facilitar la adición o modificación de manejadores.
- Mejorar la escalabilidad del sistema de verificación de solicitudes.

## Estructura

- **Cliente**: El objeto que envía la solicitud.
- **Manejador (Handler)**: Interfaz que define un método para procesar la solicitud o pasarla al siguiente manejador.
- **Manejadores Concretos**: Implementaciones del manejador que procesan la solicitud o la pasan a otro manejador.
- **Cadena de Manejadores**: Conjunto de manejadores enlazados en secuencia.

![Estructura del Patrón](https://upload.wikimedia.org/wikipedia/commons/7/76/CoR_UML_class_diagram.svg)

## Analogía en el mundo real

Imagina que en el sistema de gestión de tutorías del **Instituto Tecnológico de Tijuana**, los estudiantes envían solicitudes para la verificación de créditos. Estas solicitudes deben pasar por varias verificaciones, como la cantidad de créditos obtenidos, la validez de las actividades realizadas y la autenticidad de los documentos proporcionados.

Cada verificación puede ser tratada como un manejador. Si un manejador no puede verificar una solicitud, la pasa al siguiente hasta que se complete la validación.

## Pseudocódigo

```python
# Clase abstracta del Manejador
class Manejador:
    def __init__(self, sucesor=None):
        self._sucesor = sucesor

    def manejar(self, solicitud):
        if self._puede_manejar(solicitud):
            self._procesar(solicitud)
        elif self._sucesor:
            self._sucesor.manejar(solicitud)

    def _puede_manejar(self, solicitud):
        raise NotImplementedError

    def _procesar(self, solicitud):
        raise NotImplementedError

# Manejador concreto que verifica los créditos del semestre
class VerificadorSemestre(Manejador):
    def _puede_manejar(self, solicitud):
        return solicitud.semestre == 10

    def _procesar(self, solicitud):
        print("Solicitud verificada para el 10mo semestre.")

# Manejador concreto que verifica la cantidad de créditos
class VerificadorCreditos(Manejador):
    def _puede_manejar(self, solicitud):
        return solicitud.creditos >= 5

    def _procesar(self, solicitud):
        print("Solicitud verificada: Créditos suficientes.")

# Ejemplo de uso
solicitud = Solicitud(semestre=10, creditos=6)
cadena = VerificadorSemestre(VerificadorCreditos())
cadena.manejar(solicitud)

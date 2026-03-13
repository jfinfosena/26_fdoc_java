---
title: "Actividad Guiada: Herencia y Clases Abstractas"
description: "Aprende paso a paso cómo implementar herencia, clases abstractas y métodos abstractos mediante la creación de un sistema de nómina real."
position: 5
---

# Actividad Guiada: Construyendo un Sistema de Nómina

Bienvenido a esta actividad práctica. Hoy no solo leeremos teoría, sino que **construiremos paso a paso** un sistema de gestión de empleados para una empresa. A través de este ejercicio, entenderás por qué la herencia y la abstracción son vitales en el desarrollo de software profesional.

## Escenario de Negocio
Nuestra empresa, **"TechSoluciones"**, necesita un sistema para calcular el pago mensual de sus colaboradores. Tenemos dos tipos de empleados:
1.  **Asalariados:** Tienen un sueldo fijo mensual.
2.  **Por Horas:** Se les paga según las horas trabajadas y el valor de cada hora.

---

## Paso 1: Definir la Clase Abstracta `Empleado`

¿Por qué usar una clase **abstracta**? Porque en nuestro sistema no existe un "Empleado" genérico. Todo empleado debe ser o Asalariado o Por Horas. La clase `Empleado` servirá como una **plantilla** que define lo que todos comparten.

### El Código:
Crea un archivo llamado `Empleado.java`:

```java
public abstract class Empleado {
    private String nombre;
    private String id;

    // Constructor para inicializar atributos comunes
    public Empleado(String nombre, String id) {
        this.nombre = nombre;
        this.id = id;
    }

    // Métodos concretos (todos los empleados los usan igual)
    public String getNombre() { return nombre; }
    public String getId() { return id; }

    // MÉTODO ABSTRACTO: Cada tipo de empleado calculará su salario de forma distinta
    public abstract double calcularSalario();

    // Método concreto que usa el método abstracto (Polimorfismo en acción)
    public void mostrarReciboPago() {
        System.out.println("--- RECIBO DE PAGO ---");
        System.out.println("Empleado: " + nombre + " (ID: " + id + ")");
        System.out.println("Sueldo a pagar: $" + calcularSalario());
        System.out.println("----------------------");
    }
}
```

### Conceptos Clave:
*   **`abstract class`**: Evita que alguien haga `new Empleado(...)`. Es una idea, no un objeto real.
*   **`abstract double calcularSalario()`**: Es una **promesa**. Decimos: "Todo empleado sabe calcular su salario, pero solo sus hijos saben CÓMO hacerlo".

---

## Paso 2: Implementar la Herencia con `EmpleadoAsalariado`

Ahora crearemos la primera clase concreta. Esta clase **extiende** a `Empleado`, lo que significa que hereda su nombre e ID, pero debe cumplir la promesa de calcular el salario.

### El Código:
Crea `EmpleadoAsalariado.java`:

```java
public class EmpleadoAsalariado extends Empleado {
    private double sueldoMensual;

    public EmpleadoAsalariado(String nombre, String id, double sueldoMensual) {
        // Llamamos al constructor del padre usando super
        super(nombre, id);
        this.sueldoMensual = sueldoMensual;
    }

    // Cumpliendo la promesa del padre
    @Override
    public double calcularSalario() {
        // Lógica de negocio: Al asalariado se le paga su sueldo fijo
        return sueldoMensual;
    }
}
```

### Conceptos Clave:
*   **`extends`**: Indica que `EmpleadoAsalariado` es un tipo de `Empleado`.
*   **`super(nombre, id)`**: Envía los datos al constructor de la clase padre para que ella se encargue de guardarlos.
*   **`@Override`**: Le dice al compilador: "Estoy implementando/sobrescribiendo el método que mi padre definió".

---

## Paso 3: Aplicar Lógica de Negocio con `EmpleadoPorHoras`

Aquí es donde la abstracción brilla. Un empleado por horas tiene una lógica de cálculo totalmente diferente.

### El Código:
Crea `EmpleadoPorHoras.java`:

```java
public class EmpleadoPorHoras extends Empleado {
    private int horasTrabajadas;
    private double valorHora;

    public EmpleadoPorHoras(String nombre, String id, double valorHora) {
        super(nombre, id);
        this.valorHora = valorHora;
        this.horasTrabajadas = 0; // Inicia en 0
    }

    public void setHorasTrabajadas(int horas) {
        this.horasTrabajadas = horas;
    }

    @Override
    public double calcularSalario() {
        // Lógica de negocio: pago por productividad
        return horasTrabajadas * valorHora;
    }
}
```

---

## Paso 4: Lógica de Negocio Avanzada (Opcional)

Imagina que la empresa decide que los empleados por horas que trabajen más de 40 horas reciben un bono por horas extra. Solo tenemos que modificar la lógica dentro de `EmpleadoPorHoras`:

```java
@Override
public double calcularSalario() {
    if (horasTrabajadas > 40) {
        int extras = horasTrabajadas - 40;
        return (40 * valorHora) + (extras * valorHora * 1.5); // Pago 50% más por extra
    }
    return horasTrabajadas * valorHora;
}
```

---

## Paso 5: Probando el Sistema (Polimorfismo)

El polimorfismo nos permite tratar a diferentes objetos como si fueran del mismo tipo (su padre).

### El Código:
Crea `Main.java`:

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        // Creamos una lista de Empleados (el padre)
        ArrayList<Empleado> nomina = new ArrayList<>();

        // Agregamos hijos de diferentes tipos
        EmpleadoAsalariado emp1 = new EmpleadoAsalariado("Juan Perez", "A101", 3500000);
        
        EmpleadoPorHoras emp2 = new EmpleadoPorHoras("Maria Lopez", "H202", 25000);
        emp2.setHorasTrabajadas(45); // Trabajó horas extra

        nomina.add(emp1);
        nomina.add(emp2);

        // Procesamos la nómina de forma genérica
        System.out.println("PROCESANDO NÓMINA DE TECHSOLUCIONES\n");
        
        for (Empleado e : nomina) {
            // No importa si es asalariado o por horas,
            // Java sabe qué 'calcularSalario' llamar.
            e.mostrarReciboPago();
            System.out.println();
        }
    }
}
```

---

## Resumen de Aprendizaje

1.  **Clases Abstractas:** Sirven para definir el "qué" pero no el "cómo". No se pueden instanciar.
2.  **Herencia (`extends`):** Permite reutilizar código (`nombre`, `id`) y establecer jerarquías.
3.  **Métodos Abstractos:** Obligan a las clases hijas a implementar una lógica específica.
4.  **Polimorfismo:** Nos permite manejar una lista de `Empleado` y dejar que cada uno se comporte según su verdadera naturaleza en tiempo de ejecución.

---

## Actividad de Práctica: Sistema CRUD de Empleados

Ahora que has aprendido los fundamentos de herencia y polimorfismo, es momento de aplicarlos en un proyecto real. Tu tarea es expandir el sistema de nómina para que sea interactivo.

### Requerimientos de la Actividad:

1.  **Modelo de Datos:**
    *   Utiliza la jerarquía de clases (`Empleado`, `EmpleadoAsalariado`, `EmpleadoPorHoras`) que construimos en la guía.
    *   Agrega un nuevo tipo de empleado: **`EmpleadoComisionista`**.
        *   *Sugerencia:* Este empleado podría tener un sueldo base y un porcentaje de comisión sobre las ventas totales realizadas. Piensa en qué atributos adicionales necesitará y cómo se calculará su salario final.

2.  **Interfaz de Consola:**
    *   Implementa un menú interactivo mediante la consola que permita al usuario realizar las siguientes acciones de forma repetitiva hasta que decida salir:
        *   **1. Ingresar Empleado:** Permitir elegir el tipo de empleado y capturar sus datos (ID, Nombre, Sueldo/Valor Hora/Ventas).
        *   **2. Ver Todos los Empleados:** Mostrar la lista de todos los empleados registrados utilizando polimorfismo para mostrar sus recibos de pago.
        *   **3. Actualizar Empleado:** Buscar un empleado por su ID y permitir modificar sus datos básicos o de salario.
        *   **4. Eliminar Empleado:** Buscar un empleado por su ID y removerlo del sistema.
        *   **5. Salir.**

3.  **Restricciones Técnicas:**
    *   Utiliza un `ArrayList<Empleado>` para almacenar los objetos en memoria.
    *   Asegúrate de manejar correctamente la entrada de datos con la clase `Scanner`.
    *   Aplica buenas prácticas de programación: nombres de variables claros, métodos con una sola responsabilidad y modularización del código.

---

## Resumen de Aprendizaje

1.  **Clases Abstractas:** Sirven para definir el "qué" pero no el "cómo". No se pueden instanciar.
2.  **Herencia (`extends`):** Permite reutilizar código (`nombre`, `id`) y establecer jerarquías.
3.  **Métodos Abstractos:** Obligan a las clases hijas a implementar una lógica específica.
4.  **Polimorfismo:** Nos permite manejar una lista de `Empleado` y dejar que cada uno se comporte según su verdadera naturaleza en tiempo de ejecución.

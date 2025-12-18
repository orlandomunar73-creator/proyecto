// ===============================
// BOT INTELIGENTE DE MENSAJES
// ===============================

// "Base de datos" simple de empleados
const empleados = [
  { id: 1, nombre: "Ana", area: "soporte", email: "ana@empresa.com" },
  { id: 2, nombre: "Luis", area: "ventas", email: "luis@empresa.com" },
  { id: 3, nombre: "Marta", area: "finanzas", email: "marta@empresa.com" },
  { id: 4, nombre: "Carlos", area: "rrhh", email: "carlos@empresa.com" }
];

// Reglas simples de clasificación por palabras clave
const reglas = [
  {
    area: "soporte",
    palabrasClave: ["error", "no funciona", "fallo", "soporte", "problema"]
  },
  {
    area: "ventas",
    palabrasClave: ["precio", "cotización", "comprar", "venta", "descuento"]
  },
  {
    area: "finanzas",
    palabrasClave: ["factura", "pago", "cobro", "transferencia", "contabilidad"]
  },
  {
    area: "rrhh",
    palabrasClave: ["vacaciones", "nómina", "contrato", "licencia", "postular"]
  }
];

// Función que analiza el mensaje y decide área
function clasificarMensaje(texto) {
  const textoMin = texto.toLowerCase();

  for (const regla of reglas) {
    for (const palabra of regla.palabrasClave) {
      if (textoMin.includes(palabra)) {
        return regla.area;
      }
    }
  }

  // Si no encuentra nada específico, lo manda a soporte por defecto
  return "soporte";
}

// Función que elige un empleado del área
function seleccionarEmpleado(area) {
  const candidatos = empleados.filter(emp => emp.area === area);
  if (candidatos.length === 0) {
    return null;
  }

  // Estrategia simple: el primero de la lista (podrías rotar o balancear carga)
  return candidatos[0];
}

// Simulación de envío de mensaje a un empleado
function reenviarMensaje(mensaje, empleado) {
  console.log("=======================================");
  console.log("📨 NUEVO MENSAJE EN LA EMPRESA");
  console.log("De:    ", mensaje.remitente);
  console.log("Texto: ", mensaje.texto);
  console.log("---------------------------------------");
  console.log("🤖 BOT: Analizando mensaje...");
  console.log("Área detectada:", mensaje.areaDetectada);
  console.log("Empleado asignado:", empleado.nombre, `(${empleado.email})`);
  console.log("Acción: reenviar mensaje al correo interno del empleado.");
  console.log("=======================================\n");
}

// Función principal del bot
function procesarMensaje(mensaje) {
  // 1. Clasificar por contenido
  const area = clasificarMensaje(mensaje.texto);
  mensaje.areaDetectada = area;

  // 2. Elegir empleado
  const empleado = seleccionarEmpleado(area);

  if (!empleado) {
    console.log("No se encontró empleado para el área:", area);
    return;
  }

  // 3. Reenviar (simulado)
  reenviarMensaje(mensaje, empleado);
}

// ===============================
// PRUEBA: lista de mensajes de ejemplo
// ===============================

const mensajesEntrantes = [
  {
    id: 101,
    remitente: "cliente1@correo.com",
    texto: "Buenos días, tengo un problema: el sistema no funciona desde ayer."
  },
  {
    id: 102,
    remitente: "cliente2@correo.com",
    texto: "Quisiera saber el precio y una cotización del plan empresarial."
  },
  {
    id: 103,
    remitente: "proveedor@correo.com",
    texto: "Adjuntamos la factura del último pago pendiente."
  },
  {
    id: 104,
    remitente: "candidato@correo.com",
    texto: "Me gustaría postular a una vacante y conocer las condiciones del contrato."
  },
  {
    id: 105,
    remitente: "cliente3@correo.com",
    texto: "Hola, solo quiero hacer una consulta general, nada urgente."
  }
];

// Ejecutar el bot sobre cada mensaje
for (const mensaje of mensajesEntrantes) {
  procesarMensaje(mensaje);
}

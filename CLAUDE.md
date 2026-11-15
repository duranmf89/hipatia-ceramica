# Hipatia Cerámica: contexto para Claude Code

## Qué es

App web instalable (PWA) para Hipatia, el taller de cerámica de Pauli (Paula) en Barrio Urca, Córdoba. Tiene tres tipos de usuaria:

- **Alumnas:** reservan y cambian clases, siguen sus piezas, pagan la cuota y compran en la tienda.
- **Pauli (dueña):** agenda, asistencia, alumnas, cobros, tienda, ajustes y avisos.
- **Público sin cuenta:** compra en la tienda, pide encargos y reserva una clase de prueba.

## Quién decide qué

- **Francisco:** desarrolla y coordina.
- **Sasha:** tuvo la idea y diseñó la maqueta. Decide diseño y flujos. Trabaja al lado de Francisco en las sesiones.
- **Pauli:** decide la política del taller (precios, recargos, deudas, pausas, envíos).

Nunca inventes una política del taller. Si una decisión es de Pauli, anotala como pregunta para ella en `DECISIONES.md` y seguí con lo demás.

## Estado actual

- **Fase 0 terminada:** publicada en https://duranmf89.github.io/hipatia-ceramica/ e instalable en el celular.
- **Fase 1 en curso:** recorrer todos los flujos, corregir lo que no cierra y responder las preguntas abiertas.
- Todo vive en `index.html`: HTML, CSS y JS en un solo archivo. Los datos de prueba se guardan en `localStorage` con la clave `hipatia-demo` (versión 5) y "Empezar de cero" los reinicia.
- Login, Mercado Pago y WhatsApp están **simulados**. No hay servidor.
- `manifest.webmanifest`, `sw.js` y los `icon-*.png` sirven para instalarla en el celular. Las etiquetas del `<head>` que los enlazan y el script que registra `sw.js` antes de `</body>` hay que conservarlos.

## Documentos de referencia (abrirlos en el navegador)

- **Plan, fases y presupuesto:** https://claude.ai/artifact/2baHTdUtRaDf9edoAU1ffy. Tiene las pestañas "Registro de cambios" y "Reglas y preguntas".
- **Maqueta de Sasha:** https://claude.ai/artifact/Dg2YvBAABVY4kgADbPCBYH. Desde que empezó la Fase 1, la versión oficial es la de este repositorio y la maqueta de Sasha queda para probar ideas nuevas.

Si no podés abrir estos links, pedile a Francisco que pegue lo que haga falta.

## Cómo trabajamos

Sesiones de fin de semana con Francisco y Sasha juntos. Se recorre flujo por flujo, viendo las pantallas y decidiendo sobre la marcha.

1. Levantá la app: `python3 -m http.server 8000` y abrí http://localhost:8000. Probá en ancho de celular (390 px) y de computadora.
2. Recorré el flujo paso a paso y mostrá capturas de cada pantalla.
3. Marcá lo que no cierra: botones sin salida, reglas que se contradicen, casos borde.
4. **Preguntá antes de decidir.** Proponé una opción concreta y esperá la respuesta.
5. Hacé cambios chicos: un commit por flujo o por decisión, con el mensaje en español.
6. Anotá cada decisión en `DECISIONES.md`. Un job semanal las pasa a la pestaña "Reglas y preguntas" del plan.

### Orden de los flujos

1. **Alumna:** entrar, reservar, cambiar de día, cancelar, recuperar.
2. **Alumna:** pagar (transferencia y Mercado Pago), cuenta mes a mes, pedir pausa, piezas.
3. **Pauli:** Hoy, asistencia, agenda, cerrar un día.
4. **Pauli:** alumnas, ficha, registrar pago, pausar, no cobrar, cobros y comprobantes, renovación mensual.
5. **Tienda y tallercitos**, con cuenta y sin cuenta, y encargos.
6. **Ajustes y recordatorios.**

## Reglas ya definidas en la maqueta

- Cuota de $45.000: 4 clases de 2 horas por mes y 1 pieza mediana incluida.
- Piezas extra: chica $2.000, mediana $3.500, grande $5.000. Clase suelta: $13.000.
- Se puede cambiar de día hasta 24 h antes. Si avisa a tiempo, la clase queda para recuperar hasta fin del mes siguiente.
- La cuota se paga del 1 al 10 de cada mes. La pausa se pide antes del día 1 y ese mes no se cobra.
- Los abonos se generan solos el día 1, salteando a las alumnas en pausa.
- Recordatorios por WhatsApp: clase (el día anterior), pago (días 5 y 10), pieza lista y taller cerrado.

## Contradicciones para corregir en la Fase 1

- **C1:** al cancelar, la app siempre devuelve la clase, pero la regla dice que si no avisa a tiempo se pierde.
- **C2:** faltar sin avisar no descuenta la clase.
- **C6:** los recuperos "vencen", pero la regla de cierre dice que "siempre se recuperan".
- **P1:** el mes en curso figura "Debe" desde el día 1 aunque se puede pagar hasta el 10. Propuesta: "Por pagar" hasta el 10 y "Debe" desde el 11.
- **P2:** "Pausar" un cobro en la ficha no pausa las clases de ese mes.

## Preguntas abiertas (detalle en la pestaña "Reglas y preguntas")

- **Clases (C):** C1 cancelación tarde · C2 ausencias · C3 ¿el turno fijo reserva solo? · C4 meses con 5 semanas · C5 clases sin usar a fin de mes · C6 vencimiento de recuperos por cierre · C7 lista de espera · C8 cupos para recuperar.
- **Pagos (P):** P1 Por pagar / Debe · P2 pausar un cobro · P3 bloquear reservas a quien debe · P4 ¿la deuda vieja se actualiza al precio actual? · P5 recargo · P6 pagos parciales · P7 alumna que empieza a mitad de mes · P8 adelantar meses.
- **Pausas (PA):** PA1 mes ya pagado · PA2 guardar el turno · PA3 duración máxima · PA4 recuperos durante la pausa · PA5 pedido después del día 1.
- **Renovación (R):** R1 ¿la pieza incluida se acumula? · R2 alumnas con 2 o más meses de deuda.
- **WhatsApp (W):** W1 el aviso de cierre llega siempre · W2 recordatorios después del 10 · W3 ¿los avisos generales van por WhatsApp?
- **Tienda (T):** T1 reservar una pieza mientras se confirma la transferencia · T2 envíos al interior · T3 cancelar un tallercito.

## Fallas técnicas que se resuelven al construir (fases 2 y 3)

Estado por reserva (reservada, asistió, ausente, cancelada, cerrada) · saldo de clases por mes · la clase suelta como crédito de un solo uso · controlar el límite de horas al cambiar de día · liberar reservas al aprobar una pausa · guardar la fecha de vencimiento de los recuperos · usar fechas reales en lugar de noviembre fijo.

## Próximas fases

- **Fase 2:** Supabase (cuentas, base de datos, fotos).
- **Fase 3:** conectar las pantallas a los datos reales.
- **Fase 4:** Mercado Pago Checkout Pro.
- **Fase 5:** WhatsApp Cloud API de Meta.
- **Fase 6:** piloto con Pauli y algunas alumnas.

## Límites

- **Este repositorio es público:** nunca subas claves (Mercado Pago, Meta, Supabase service key) ni datos reales de alumnas. Las claves van en el servidor.
- No cambies precios ni políticas sin una decisión anotada.
- Si una sesión cambia algo que se ve en pantalla, contalo en `DECISIONES.md` para que Sasha y Pauli lo sepan.

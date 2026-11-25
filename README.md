# Watchdog de Certificados TLS/SSL

[![Alcance de la Verificación Perimetral](https://img.shields.io/badge/TAVARDT-Diagn%C3%B3stico%20T%C3%A9cnico-5b3cf5.svg)](https://tools.tavardt.com/mx/analisis-cabeceras-seguridad/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Monitoreo automatizado de caducidad de certificados SSL/TLS, integridad de cadena criptográfica y transporte seguro en México.

Desarrollado y mantenido por la práctica de ciberdefensa de **TAVARDT** para organizaciones mexicanas.

***

## 1. Alcance de la Verificación Perimetral

Esta herramienta de terminal audita el ciclo de vida de certificados, suites de cifrado y cadenas de confianza:

* **Alerta Preventiva de Caducidad:** Calcula los días restantes hasta la expiración con umbrales automatizados (> 30 días válido, <= 30 días advertencia, <= 0 días expirado).
* **Auditoría de Versión de Protocolo:** Verifica la imposición de TLS 1.3 y TLS 1.2 conforme a las recomendaciones del INAI y LFPDPPP.
* **Evaluación de Suite Criptográfica:** Inspecciona los algoritmos de cifrado negociados y la longitud de clave simétrica (128/256 bits).
* **Validación de Autoridad de Certificación (CA):** Inspecciona la organización emisora en la cadena de confianza y los nombres alternativos del sujeto (SAN).
* **Cero Dependencias Externas:** Funciona íntegramente con los módulos nativos `ssl` y `socket` de la biblioteca estándar de Python.

***

## 2. Matriz de Línea Base Criptográfica y Ciclo de Vida

<table>
  <thead>
    <tr>
      <th>Parámetro Inspeccionado</th>
      <th>Método de Evaluación en el Handshake</th>
      <th>Línea Base Recomendada</th>
      <th>Riesgo Operativo Mitigado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Caducidad del Certificado</strong></td>
      <td>Compara el registro <code>notAfter</code> con la hora UTC actual</td>
      <td>Alerta preventiva con &gt; 30 días restantes</td>
      <td>Interrupción de servicio y advertencias de sitio no seguro</td>
    </tr>
    <tr>
      <td><strong>Versión del Protocolo</strong></td>
      <td>Inspecciona la versión negociada mediante <code>ssock.version()</code></td>
      <td><code>TLSv1.3</code> preferente, <code>TLSv1.2</code> mínimo</td>
      <td>Ataques de degradación de protocolo y cifrados obsoletos</td>
    </tr>
    <tr>
      <td><strong>Suite de Cifrado y Bits</strong></td>
      <td>Extrae parámetros criptográficos mediante <code>ssock.cipher()</code></td>
      <td>Cifrados modernos AEAD con claves de &gt;= 128 bits</td>
      <td>Intercepción man-in-the-middle y ruptura de cifrados débiles</td>
    </tr>
    <tr>
      <td><strong>Autoridad de Certificación (CA)</strong></td>
      <td>Obtiene la organización emisora de la cadena de confianza</td>
      <td>CA pública válida reconocida por el almacén del sistema</td>
      <td>Uso de certificados autofirmados no confiables</td>
    </tr>
    <tr>
      <td><strong>Subject Common Name (CN)</strong></td>
      <td>Valida el nombre de host frente al sujeto del certificado</td>
      <td>Coincidencia exacta con el FQDN o wildcard SAN</td>
      <td>Errores de discrepancia de nombre de host y suplantación</td>
    </tr>
  </tbody>
</table>

***

## 3. Guía de Inicio Rápido y Automatización

Ejecute esta herramienta directamente mediante Python sin necesidad de librerías de terceros:

```bash
python watchdog.py ejemplo.com.mx
```

El motor de auditoría evalúa el handshake de socket de forma determinista:

* **Librería Estándar Nativa:** Funciona exclusivamente con las bibliotecas estándar de Python (`ssl`, `socket`, `datetime`), requiriendo cero dependencias externas.
* **Diagnóstico en Tres Niveles:** Clasifica el estado del certificado como PASS (> 30 días), WARN (próximo a caducar) o FAIL (expirado).
* **Compatibilidad con CI/CD:** Se integra con facilidad en etapas de despliegue continuo y validaciones de seguridad perimetral.

***

## 4. Preguntas Frecuentes (FAQ Técnico)

### 1. ¿Por qué resulta crítico un umbral de alerta de 30 días antes de la caducidad del certificado?
Una alerta preventiva a 30 días de la expiración proporciona margen operativo para ejecutar renovaciones automáticas mediante ACME o rotaciones manuales. Superar esta fecha provoca que los navegadores bloqueen inmediatamente el acceso con advertencias de seguridad.

### 2. ¿Cómo inspecciona watchdog.py el handshake TLS sin descargar cargas HTTP completas?
La herramienta establece una conexión directa por socket TCP en el puerto 443 encapsulada con TLS nativo (`ssl.wrap_socket`). Extrae el certificado y los parámetros criptográficos durante el handshake, logrando diagnósticos rápidos con consumo mínimo de red.

### 3. ¿Qué riesgos de seguridad implican las versiones obsoletas como TLS 1.0 y TLS 1.1?
Los protocolos antiguos utilizan algoritmos vulnerables a ataques de degradación e intercepción man-in-the-middle. La imposición de TLS 1.2 o TLS 1.3 garantiza confidencialidad directa (forward secrecy) y algoritmos modernos de cifrado autenticado.

### 4. ¿Cómo gestiona el script tanto nombres de dominio limpios como URLs completas?
La herramienta procesa el argumento introducido mediante `urllib.parse`. Si el objetivo comienza con `http://` o `https://`, extrae limpiamente el hostname para iniciar la conexión por socket TCP en el puerto 443.

### 5. ¿Puede este watchdog ejecutarse en flujos de CI/CD y tareas programadas (cron)?
Sí, `watchdog.py` opera nativamente en Linux, macOS y Windows sin requerir paquetes externos. Su salida determinista facilita la integración en GitHub Actions, cron jobs y sistemas automatizados de supervisión de disponibilidad.

***

## 5. Herramientas Técnicas Online (México)

Audite su infraestructura pública mediante la suite de diagnóstico de TAVARDT:

* [Validador de Consent Mode v2 y LFPDPPP](https://tools.tavardt.com/mx/validador-consent-mode-v2-lfpdppp/)
* [Verificador DMARC contra Suplantación](https://tools.tavardt.com/mx/verificador-dmarc-correo/)
* [Escáner de Archivos Sensibles Expuestos](https://tools.tavardt.com/mx/escaneo-archivos-expuestos/)
* [Portal de Diagnóstico Técnico México](https://tools.tavardt.com/mx/)

***

## 6. Consultoría y Protección Criptográfica en México

Para auditoría criptográfica, remediación de vulnerabilidades perimetrales y consultoría en ciberseguridad en Ciudad de México, Monterrey o Guadalajara, contacte con **TAVARDT**.

***

## Enlaces Útiles

* **Portal Corporativo:** https://ag.tavardt.com/mx/
* **Suite de Diagnóstico:** https://tools.tavardt.com/mx/
* **Contacto Técnico:** https://ag.tavardt.com/mx/contacto/

***

### Licencia
Distribuido bajo la Licencia MIT.

***

© 2026 TAVARDT. Todos los derechos reservados.

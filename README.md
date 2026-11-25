# Watchdog de Certificados TLS/SSL

[![Licencia: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Alcance de la Verificación Perimetral](https://img.shields.io/badge/TAVARDT-Diagn%C3%B3stico%20T%C3%A9cnico-5b3cf5.svg)](https://tools.tavardt.com/mx/analisis-cabeceras-seguridad/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Monitoreo automatizado de expiración de certificados SSL/TLS, cadena criptográfica y seguridad perimetral de datos en México.

Desarrollado y mantenido por el equipo de ciberdefensa y protección de datos de **TAVARDT**.

***

## 1. Alcance de la Verificación Perimetral

Esta utilidad de consola evalúa la vigencia, protocolos y robustez criptográfica de los certificados SSL/TLS:

* **Alerta Preventiva de Expiración:** Calcula los días útiles previos al vencimiento del certificado digital.
* **Auditoría de Cifras e Protocolos:** Verifica la implementación de TLS 1.3 y la desactivación de suites débiles.
* **Validación de Autoridad Emisora (CA):** Inspecciona la raíz de confianza y la validez de los nombres de dominio (SAN).
* **Cero Dependencias Externas:** Opera exclusivamente sobre las librerías nativas ssl y socket de Python.

***

## 2. Guía de Ejecución Rápida

Ejecute la herramienta directamente en Python sin requerir librerías externas adicionales:

```bash
python watchdog.py ejemplo.mx
```

***

## 3. Herramientas Técnicas Online (México)

Audite la seguridad perimetral de sus sistemas web con las herramientas oficiales de TAVARDT:

* Verificador de Cabeceras de Seguridad
* [Validador de Consent Mode v2 y LFPDPPP](https://tools.tavardt.com/mx/validador-consent-mode-v2-lfpdppp/)
* [Verificador DMARC contra Suplantación](https://tools.tavardt.com/mx/verificador-dmarc-correo/)
* [Escáner de Archivos Sensibles Expuestos](https://tools.tavardt.com/mx/escaneo-archivos-expuestos/)
* [Portal de Diagnóstico Técnico México](https://tools.tavardt.com/mx/)

***

## 4. Consultoría y Protección Perimetral

Para hardening de infraestructura, renovación de llaves y ciberseguridad corporativa en México, contacte con **TAVARDT**.

***

## Enlaces Útiles

* **Portal Corporativo:** https://ag.tavardt.com/mx/
* **Suite de Diagnósticos:** https://tools.tavardt.com/mx/
* **Contacto Técnico:** https://ag.tavardt.com/mx/contacto/

***

### Licencia
Distribuido bajo la Licencia MIT. Consulte el archivo [LICENSE](LICENSE) para más detalles.

***

© 2026 TAVARDT. Todos los derechos reservados.
